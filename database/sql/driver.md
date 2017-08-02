
 Package driver

    import "database/sql/driver"

    Overview
    Index

Overview ▾

Package driver defines interfaces to be implemented by database drivers as used by package sql.

Most code should use package sql.
Index ▾

    Variables
    func IsScanValue(v interface{}) bool
    func IsValue(v interface{}) bool
    type ColumnConverter
    type Conn
    type ConnBeginTx
    type ConnPrepareContext
    type Driver
    type Execer
    type ExecerContext
    type IsolationLevel
    type NamedValue
    type NotNull
        func (n NotNull) ConvertValue(v interface{}) (Value, error)
    type Null
        func (n Null) ConvertValue(v interface{}) (Value, error)
    type Pinger
    type Queryer
    type QueryerContext
    type Result
    type Rows
    type RowsAffected
        func (RowsAffected) LastInsertId() (int64, error)
        func (v RowsAffected) RowsAffected() (int64, error)
    type RowsColumnTypeDatabaseTypeName
    type RowsColumnTypeLength
    type RowsColumnTypeNullable
    type RowsColumnTypePrecisionScale
    type RowsColumnTypeScanType
    type RowsNextResultSet
    type Stmt
    type StmtExecContext
    type StmtQueryContext
    type Tx
    type TxOptions
    type Value
    type ValueConverter
    type Valuer

Package files

driver.go types.go
Variables

Bool is a ValueConverter that converts input values to bools.

The conversion rules are:

- booleans are returned unchanged
- for integer types,
     1 is true
     0 is false,
     other integers are an error
- for strings and []byte, same rules as strconv.ParseBool
- all other types are an error

var Bool boolType

DefaultParameterConverter is the default implementation of ValueConverter that's used when a Stmt doesn't implement ColumnConverter.

DefaultParameterConverter returns its argument directly if IsValue(arg). Otherwise, if the argument implements Valuer, its Value method is used to return a Value. As a fallback, the provided argument's underlying type is used to convert it to a Value: underlying integer types are converted to int64, floats to float64, bool, string, and []byte to themselves. If the argument is a nil pointer, ConvertValue returns a nil Value. If the argument is a non-nil pointer, it is dereferenced and ConvertValue is called recursively. Other types are an error.

var DefaultParameterConverter defaultConverter

ErrBadConn should be returned by a driver to signal to the sql package that a driver.Conn is in a bad state (such as the server having earlier closed the connection) and the sql package should retry on a new connection.

To prevent duplicate operations, ErrBadConn should NOT be returned if there's a possibility that the database server might have performed the operation. Even if the server sends back an error, you shouldn't return ErrBadConn.

var ErrBadConn = errors.New("driver: bad connection")

ErrSkip may be returned by some optional interfaces' methods to indicate at runtime that the fast path is unavailable and the sql package should continue as if the optional interface was not implemented. ErrSkip is only supported where explicitly documented.

var ErrSkip = errors.New("driver: skip fast-path; continue as if unimplemented")

Int32 is a ValueConverter that converts input values to int64, respecting the limits of an int32 value.

var Int32 int32Type

ResultNoRows is a pre-defined Result for drivers to return when a DDL command (such as a CREATE TABLE) succeeds. It returns an error for both LastInsertId and RowsAffected.

var ResultNoRows noRows

String is a ValueConverter that converts its input to a string. If the value is already a string or []byte, it's unchanged. If the value is of another type, conversion to string is done with fmt.Sprintf("%v", v).

var String stringType

func IsScanValue

func IsScanValue(v interface{}) bool

IsScanValue is equivalent to IsValue. It exists for compatibility.
func IsValue

func IsValue(v interface{}) bool

IsValue reports whether v is a valid Value parameter type.
type ColumnConverter

ColumnConverter may be optionally implemented by Stmt if the statement is aware of its own columns' types and can convert from any type to a driver Value.

type ColumnConverter interface {
        // ColumnConverter returns a ValueConverter for the provided
        // column index. If the type of a specific column isn't known
        // or shouldn't be handled specially, DefaultValueConverter
        // can be returned.
        ColumnConverter(idx int) ValueConverter
}

type Conn

Conn is a connection to a database. It is not used concurrently by multiple goroutines.

Conn is assumed to be stateful.

type Conn interface {
        // Prepare returns a prepared statement, bound to this connection.
        Prepare(query string) (Stmt, error)

        // Close invalidates and potentially stops any current
        // prepared statements and transactions, marking this
        // connection as no longer in use.
        //
        // Because the sql package maintains a free pool of
        // connections and only calls Close when there's a surplus of
        // idle connections, it shouldn't be necessary for drivers to
        // do their own connection caching.
        Close() error

        // Begin starts and returns a new transaction.
        //
        // Deprecated: Drivers should implement ConnBeginTx instead (or additionally).
        Begin() (Tx, error)
}

type ConnBeginTx

ConnBeginTx enhances the Conn interface with context and TxOptions.

type ConnBeginTx interface {
        // BeginTx starts and returns a new transaction.
        // If the context is canceled by the user the sql package will
        // call Tx.Rollback before discarding and closing the connection.
        //
        // This must check opts.Isolation to determine if there is a set
        // isolation level. If the driver does not support a non-default
        // level and one is set or if there is a non-default isolation level
        // that is not supported, an error must be returned.
        //
        // This must also check opts.ReadOnly to determine if the read-only
        // value is true to either set the read-only transaction property if supported
        // or return an error if it is not supported.
        BeginTx(ctx context.Context, opts TxOptions) (Tx, error)
}

type ConnPrepareContext

ConnPrepareContext enhances the Conn interface with context.

type ConnPrepareContext interface {
        // PrepareContext returns a prepared statement, bound to this connection.
        // context is for the preparation of the statement,
        // it must not store the context within the statement itself.
        PrepareContext(ctx context.Context, query string) (Stmt, error)
}

type Driver

Driver is the interface that must be implemented by a database driver.

type Driver interface {
        // Open returns a new connection to the database.
        // The name is a string in a driver-specific format.
        //
        // Open may return a cached connection (one previously
        // closed), but doing so is unnecessary; the sql package
        // maintains a pool of idle connections for efficient re-use.
        //
        // The returned connection is only used by one goroutine at a
        // time.
        Open(name string) (Conn, error)
}

type Execer

Execer is an optional interface that may be implemented by a Conn.

If a Conn does not implement Execer, the sql package's DB.Exec will first prepare a query, execute the statement, and then close the statement.

Exec may return ErrSkip.

Deprecated: Drivers should implement ExecerContext instead (or additionally).

type Execer interface {
        Exec(query string, args []Value) (Result, error)
}

type ExecerContext

ExecerContext is an optional interface that may be implemented by a Conn.

If a Conn does not implement ExecerContext, the sql package's DB.Exec will first prepare a query, execute the statement, and then close the statement.

ExecerContext may return ErrSkip.

ExecerContext must honor the context timeout and return when the context is canceled.

type ExecerContext interface {
        ExecContext(ctx context.Context, query string, args []NamedValue) (Result, error)
}

type IsolationLevel

IsolationLevel is the transaction isolation level stored in TxOptions.

This type should be considered identical to sql.IsolationLevel along with any values defined on it.

type IsolationLevel int

type NamedValue

NamedValue holds both the value name and value.

type NamedValue struct {
        // If the Name is not empty it should be used for the parameter identifier and
        // not the ordinal position.
        //
        // Name will not have a symbol prefix.
        Name string

        // Ordinal position of the parameter starting from one and is always set.
        Ordinal int

        // Value is the parameter value.
        Value Value
}

type NotNull

NotNull is a type that implements ValueConverter by disallowing nil values but otherwise delegating to another ValueConverter.

type NotNull struct {
        Converter ValueConverter
}

func (NotNull) ConvertValue

func (n NotNull) ConvertValue(v interface{}) (Value, error)

type Null

Null is a type that implements ValueConverter by allowing nil values but otherwise delegating to another ValueConverter.

type Null struct {
        Converter ValueConverter
}

func (Null) ConvertValue

func (n Null) ConvertValue(v interface{}) (Value, error)

type Pinger

Pinger is an optional interface that may be implemented by a Conn.

If a Conn does not implement Pinger, the sql package's DB.Ping and DB.PingContext will check if there is at least one Conn available.

If Conn.Ping returns ErrBadConn, DB.Ping and DB.PingContext will remove the Conn from pool.

type Pinger interface {
        Ping(ctx context.Context) error
}

type Queryer

Queryer is an optional interface that may be implemented by a Conn.

If a Conn does not implement Queryer, the sql package's DB.Query will first prepare a query, execute the statement, and then close the statement.

Query may return ErrSkip.

Deprecated: Drivers should implement QueryerContext instead (or additionally).

type Queryer interface {
        Query(query string, args []Value) (Rows, error)
}

type QueryerContext

QueryerContext is an optional interface that may be implemented by a Conn.

If a Conn does not implement QueryerContext, the sql package's DB.Query will first prepare a query, execute the statement, and then close the statement.

QueryerContext may return ErrSkip.

QueryerContext must honor the context timeout and return when the context is canceled.

type QueryerContext interface {
        QueryContext(ctx context.Context, query string, args []NamedValue) (Rows, error)
}

type Result

Result is the result of a query execution.

type Result interface {
        // LastInsertId returns the database's auto-generated ID
        // after, for example, an INSERT into a table with primary
        // key.
        LastInsertId() (int64, error)

        // RowsAffected returns the number of rows affected by the
        // query.
        RowsAffected() (int64, error)
}

type Rows

Rows is an iterator over an executed query's results.

type Rows interface {
        // Columns returns the names of the columns. The number of
        // columns of the result is inferred from the length of the
        // slice. If a particular column name isn't known, an empty
        // string should be returned for that entry.
        Columns() []string

        // Close closes the rows iterator.
        Close() error

        // Next is called to populate the next row of data into
        // the provided slice. The provided slice will be the same
        // size as the Columns() are wide.
        //
        // Next should return io.EOF when there are no more rows.
        Next(dest []Value) error
}

type RowsAffected

RowsAffected implements Result for an INSERT or UPDATE operation which mutates a number of rows.

type RowsAffected int64

func (RowsAffected) LastInsertId

func (RowsAffected) LastInsertId() (int64, error)

func (RowsAffected) RowsAffected

func (v RowsAffected) RowsAffected() (int64, error)

type RowsColumnTypeDatabaseTypeName

RowsColumnTypeDatabaseTypeName may be implemented by Rows. It should return the database system type name without the length. Type names should be uppercase. Examples of returned types: "VARCHAR", "NVARCHAR", "VARCHAR2", "CHAR", "TEXT", "DECIMAL", "SMALLINT", "INT", "BIGINT", "BOOL", "[]BIGINT", "JSONB", "XML", "TIMESTAMP".

type RowsColumnTypeDatabaseTypeName interface {
        Rows
        ColumnTypeDatabaseTypeName(index int) string
}

type RowsColumnTypeLength

RowsColumnTypeLength may be implemented by Rows. It should return the length of the column type if the column is a variable length type. If the column is not a variable length type ok should return false. If length is not limited other than system limits, it should return math.MaxInt64. The following are examples of returned values for various types:

TEXT          (math.MaxInt64, true)
varchar(10)   (10, true)
nvarchar(10)  (10, true)
decimal       (0, false)
int           (0, false)
bytea(30)     (30, true)

type RowsColumnTypeLength interface {
        Rows
        ColumnTypeLength(index int) (length int64, ok bool)
}

type RowsColumnTypeNullable

RowsColumnTypeNullable may be implemented by Rows. The nullable value should be true if it is known the column may be null, or false if the column is known to be not nullable. If the column nullability is unknown, ok should be false.

type RowsColumnTypeNullable interface {
        Rows
        ColumnTypeNullable(index int) (nullable, ok bool)
}

type RowsColumnTypePrecisionScale

RowsColumnTypePrecisionScale may be implemented by Rows. It should return the precision and scale for decimal types. If not applicable, ok should be false. The following are examples of returned values for various types:

decimal(38, 4)    (38, 4, true)
int               (0, 0, false)
decimal           (math.MaxInt64, math.MaxInt64, true)

type RowsColumnTypePrecisionScale interface {
        Rows
        ColumnTypePrecisionScale(index int) (precision, scale int64, ok bool)
}

type RowsColumnTypeScanType

RowsColumnTypeScanType may be implemented by Rows. It should return the value type that can be used to scan types into. For example, the database column type "bigint" this should return "reflect.TypeOf(int64(0))".

type RowsColumnTypeScanType interface {
        Rows
        ColumnTypeScanType(index int) reflect.Type
}

type RowsNextResultSet

RowsNextResultSet extends the Rows interface by providing a way to signal the driver to advance to the next result set.

type RowsNextResultSet interface {
        Rows

        // HasNextResultSet is called at the end of the current result set and
        // reports whether there is another result set after the current one.
        HasNextResultSet() bool

        // NextResultSet advances the driver to the next result set even
        // if there are remaining rows in the current result set.
        //
        // NextResultSet should return io.EOF when there are no more result sets.
        NextResultSet() error
}

type Stmt

Stmt is a prepared statement. It is bound to a Conn and not used by multiple goroutines concurrently.

type Stmt interface {
        // Close closes the statement.
        //
        // As of Go 1.1, a Stmt will not be closed if it's in use
        // by any queries.
        Close() error

        // NumInput returns the number of placeholder parameters.
        //
        // If NumInput returns >= 0, the sql package will sanity check
        // argument counts from callers and return errors to the caller
        // before the statement's Exec or Query methods are called.
        //
        // NumInput may also return -1, if the driver doesn't know
        // its number of placeholders. In that case, the sql package
        // will not sanity check Exec or Query argument counts.
        NumInput() int

        // Exec executes a query that doesn't return rows, such
        // as an INSERT or UPDATE.
        //
        // Deprecated: Drivers should implement StmtExecContext instead (or additionally).
        Exec(args []Value) (Result, error)

        // Query executes a query that may return rows, such as a
        // SELECT.
        //
        // Deprecated: Drivers should implement StmtQueryContext instead (or additionally).
        Query(args []Value) (Rows, error)
}

type StmtExecContext

StmtExecContext enhances the Stmt interface by providing Exec with context.

type StmtExecContext interface {
        // ExecContext executes a query that doesn't return rows, such
        // as an INSERT or UPDATE.
        //
        // ExecContext must honor the context timeout and return when it is canceled.
        ExecContext(ctx context.Context, args []NamedValue) (Result, error)
}

type StmtQueryContext

StmtQueryContext enhances the Stmt interface by providing Query with context.

type StmtQueryContext interface {
        // QueryContext executes a query that may return rows, such as a
        // SELECT.
        //
        // QueryContext must honor the context timeout and return when it is canceled.
        QueryContext(ctx context.Context, args []NamedValue) (Rows, error)
}

type Tx

Tx is a transaction.

type Tx interface {
        Commit() error
        Rollback() error
}

type TxOptions

TxOptions holds the transaction options.

This type should be considered identical to sql.TxOptions.

type TxOptions struct {
        Isolation IsolationLevel
        ReadOnly  bool
}

type Value

Value is a value that drivers must be able to handle. It is either nil or an instance of one of these types:

int64
float64
bool
[]byte
string
time.Time

type Value interface{}

type ValueConverter

ValueConverter is the interface providing the ConvertValue method.

Various implementations of ValueConverter are provided by the driver package to provide consistent implementations of conversions between drivers. The ValueConverters have several uses:

* converting from the Value types as provided by the sql package
  into a database table's specific column type and making sure it
  fits, such as making sure a particular int64 fits in a
  table's uint16 column.

* converting a value as given from the database into one of the
  driver Value types.

* by the sql package, for converting from a driver's Value type
  to a user's type in a scan.

type ValueConverter interface {
        // ConvertValue converts a value to a driver Value.
        ConvertValue(v interface{}) (Value, error)
}

type Valuer

Valuer is the interface providing the Value method.

Types implementing Valuer interface are able to convert themselves to a driver Value.

type Valuer interface {
        // Value returns a driver Value.
        Value() (Value, error)
}
