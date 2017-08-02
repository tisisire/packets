
 Package sql

    import "database/sql"

    Overview
    Index
    Examples
    Subdirectories

Overview ▾

Package sql provides a generic interface around SQL (or SQL-like) databases.

The sql package must be used in conjunction with a database driver. See https://golang.org/s/sqldrivers for a list of drivers.

Drivers that do not support context cancelation will not return until after the query is completed.

For usage examples, see the wiki page at https://golang.org/s/sqlwiki.
Index ▾

    Variables
    func Drivers() []string
    func Register(name string, driver driver.Driver)
    type ColumnType
        func (ci *ColumnType) DatabaseTypeName() string
        func (ci *ColumnType) DecimalSize() (precision, scale int64, ok bool)
        func (ci *ColumnType) Length() (length int64, ok bool)
        func (ci *ColumnType) Name() string
        func (ci *ColumnType) Nullable() (nullable, ok bool)
        func (ci *ColumnType) ScanType() reflect.Type
    type DB
        func Open(driverName, dataSourceName string) (*DB, error)
        func (db *DB) Begin() (*Tx, error)
        func (db *DB) BeginTx(ctx context.Context, opts *TxOptions) (*Tx, error)
        func (db *DB) Close() error
        func (db *DB) Driver() driver.Driver
        func (db *DB) Exec(query string, args ...interface{}) (Result, error)
        func (db *DB) ExecContext(ctx context.Context, query string, args ...interface{}) (Result, error)
        func (db *DB) Ping() error
        func (db *DB) PingContext(ctx context.Context) error
        func (db *DB) Prepare(query string) (*Stmt, error)
        func (db *DB) PrepareContext(ctx context.Context, query string) (*Stmt, error)
        func (db *DB) Query(query string, args ...interface{}) (*Rows, error)
        func (db *DB) QueryContext(ctx context.Context, query string, args ...interface{}) (*Rows, error)
        func (db *DB) QueryRow(query string, args ...interface{}) *Row
        func (db *DB) QueryRowContext(ctx context.Context, query string, args ...interface{}) *Row
        func (db *DB) SetConnMaxLifetime(d time.Duration)
        func (db *DB) SetMaxIdleConns(n int)
        func (db *DB) SetMaxOpenConns(n int)
        func (db *DB) Stats() DBStats
    type DBStats
    type IsolationLevel
    type NamedArg
        func Named(name string, value interface{}) NamedArg
    type NullBool
        func (n *NullBool) Scan(value interface{}) error
        func (n NullBool) Value() (driver.Value, error)
    type NullFloat64
        func (n *NullFloat64) Scan(value interface{}) error
        func (n NullFloat64) Value() (driver.Value, error)
    type NullInt64
        func (n *NullInt64) Scan(value interface{}) error
        func (n NullInt64) Value() (driver.Value, error)
    type NullString
        func (ns *NullString) Scan(value interface{}) error
        func (ns NullString) Value() (driver.Value, error)
    type RawBytes
    type Result
    type Row
        func (r *Row) Scan(dest ...interface{}) error
    type Rows
        func (rs *Rows) Close() error
        func (rs *Rows) ColumnTypes() ([]*ColumnType, error)
        func (rs *Rows) Columns() ([]string, error)
        func (rs *Rows) Err() error
        func (rs *Rows) Next() bool
        func (rs *Rows) NextResultSet() bool
        func (rs *Rows) Scan(dest ...interface{}) error
    type Scanner
    type Stmt
        func (s *Stmt) Close() error
        func (s *Stmt) Exec(args ...interface{}) (Result, error)
        func (s *Stmt) ExecContext(ctx context.Context, args ...interface{}) (Result, error)
        func (s *Stmt) Query(args ...interface{}) (*Rows, error)
        func (s *Stmt) QueryContext(ctx context.Context, args ...interface{}) (*Rows, error)
        func (s *Stmt) QueryRow(args ...interface{}) *Row
        func (s *Stmt) QueryRowContext(ctx context.Context, args ...interface{}) *Row
    type Tx
        func (tx *Tx) Commit() error
        func (tx *Tx) Exec(query string, args ...interface{}) (Result, error)
        func (tx *Tx) ExecContext(ctx context.Context, query string, args ...interface{}) (Result, error)
        func (tx *Tx) Prepare(query string) (*Stmt, error)
        func (tx *Tx) PrepareContext(ctx context.Context, query string) (*Stmt, error)
        func (tx *Tx) Query(query string, args ...interface{}) (*Rows, error)
        func (tx *Tx) QueryContext(ctx context.Context, query string, args ...interface{}) (*Rows, error)
        func (tx *Tx) QueryRow(query string, args ...interface{}) *Row
        func (tx *Tx) QueryRowContext(ctx context.Context, query string, args ...interface{}) *Row
        func (tx *Tx) Rollback() error
        func (tx *Tx) Stmt(stmt *Stmt) *Stmt
        func (tx *Tx) StmtContext(ctx context.Context, stmt *Stmt) *Stmt
    type TxOptions

Examples

    DB.Query
    DB.QueryRow
    DB.Query (MultipleResultSets)

Package files

convert.go ctxutil.go sql.go
Variables

ErrNoRows is returned by Scan when QueryRow doesn't return a row. In such a case, QueryRow returns a placeholder *Row value that defers this error until a Scan.

var ErrNoRows = errors.New("sql: no rows in result set")

ErrTxDone is returned by any operation that is performed on a transaction that has already been committed or rolled back.

var ErrTxDone = errors.New("sql: Transaction has already been committed or rolled back")

func Drivers

func Drivers() []string

Drivers returns a sorted list of the names of the registered drivers.
func Register

func Register(name string, driver driver.Driver)

Register makes a database driver available by the provided name. If Register is called twice with the same name or if driver is nil, it panics.
type ColumnType

ColumnType contains the name and type of a column.

type ColumnType struct {
        // contains filtered or unexported fields
}

func (*ColumnType) DatabaseTypeName

func (ci *ColumnType) DatabaseTypeName() string

DatabaseTypeName returns the database system name of the column type. If an empty string is returned the driver type name is not supported. Consult your driver documentation for a list of driver data types. Length specifiers are not included. Common type include "VARCHAR", "TEXT", "NVARCHAR", "DECIMAL", "BOOL", "INT", "BIGINT".
func (*ColumnType) DecimalSize

func (ci *ColumnType) DecimalSize() (precision, scale int64, ok bool)

DecimalSize returns the scale and precision of a decimal type. If not applicable or if not supported ok is false.
func (*ColumnType) Length

func (ci *ColumnType) Length() (length int64, ok bool)

Length returns the column type length for variable length column types such as text and binary field types. If the type length is unbounded the value will be math.MaxInt64 (any database limits will still apply). If the column type is not variable length, such as an int, or if not supported by the driver ok is false.
func (*ColumnType) Name

func (ci *ColumnType) Name() string

Name returns the name or alias of the column.
func (*ColumnType) Nullable

func (ci *ColumnType) Nullable() (nullable, ok bool)

Nullable returns whether the column may be null. If a driver does not support this property ok will be false.
func (*ColumnType) ScanType

func (ci *ColumnType) ScanType() reflect.Type

ScanType returns a Go type suitable for scanning into using Rows.Scan. If a driver does not support this property ScanType will return the type of an empty interface.
type DB

DB is a database handle representing a pool of zero or more underlying connections. It's safe for concurrent use by multiple goroutines.

The sql package creates and frees connections automatically; it also maintains a free pool of idle connections. If the database has a concept of per-connection state, such state can only be reliably observed within a transaction. Once DB.Begin is called, the returned Tx is bound to a single connection. Once Commit or Rollback is called on the transaction, that transaction's connection is returned to DB's idle connection pool. The pool size can be controlled with SetMaxIdleConns.

type DB struct {
        // contains filtered or unexported fields
}

func Open

func Open(driverName, dataSourceName string) (*DB, error)

Open opens a database specified by its database driver name and a driver-specific data source name, usually consisting of at least a database name and connection information.

Most users will open a database via a driver-specific connection helper function that returns a *DB. No database drivers are included in the Go standard library. See https://golang.org/s/sqldrivers for a list of third-party drivers.

Open may just validate its arguments without creating a connection to the database. To verify that the data source name is valid, call Ping.

The returned DB is safe for concurrent use by multiple goroutines and maintains its own pool of idle connections. Thus, the Open function should be called just once. It is rarely necessary to close a DB.
func (*DB) Begin

func (db *DB) Begin() (*Tx, error)

Begin starts a transaction. The default isolation level is dependent on the driver.
func (*DB) BeginTx

func (db *DB) BeginTx(ctx context.Context, opts *TxOptions) (*Tx, error)

BeginTx starts a transaction.

The provided context is used until the transaction is committed or rolled back. If the context is canceled, the sql package will roll back the transaction. Tx.Commit will return an error if the context provided to BeginTx is canceled.

The provided TxOptions is optional and may be nil if defaults should be used. If a non-default isolation level is used that the driver doesn't support, an error will be returned.
func (*DB) Close

func (db *DB) Close() error

Close closes the database, releasing any open resources.

It is rare to Close a DB, as the DB handle is meant to be long-lived and shared between many goroutines.
func (*DB) Driver

func (db *DB) Driver() driver.Driver

Driver returns the database's underlying driver.
func (*DB) Exec

func (db *DB) Exec(query string, args ...interface{}) (Result, error)

Exec executes a query without returning any rows. The args are for any placeholder parameters in the query.
func (*DB) ExecContext

func (db *DB) ExecContext(ctx context.Context, query string, args ...interface{}) (Result, error)

ExecContext executes a query without returning any rows. The args are for any placeholder parameters in the query.
func (*DB) Ping

func (db *DB) Ping() error

Ping verifies a connection to the database is still alive, establishing a connection if necessary.
func (*DB) PingContext

func (db *DB) PingContext(ctx context.Context) error

PingContext verifies a connection to the database is still alive, establishing a connection if necessary.
func (*DB) Prepare

func (db *DB) Prepare(query string) (*Stmt, error)

Prepare creates a prepared statement for later queries or executions. Multiple queries or executions may be run concurrently from the returned statement. The caller must call the statement's Close method when the statement is no longer needed.
func (*DB) PrepareContext

func (db *DB) PrepareContext(ctx context.Context, query string) (*Stmt, error)

PrepareContext creates a prepared statement for later queries or executions. Multiple queries or executions may be run concurrently from the returned statement. The caller must call the statement's Close method when the statement is no longer needed.

The provided context is used for the preparation of the statement, not for the execution of the statement.
func (*DB) Query

func (db *DB) Query(query string, args ...interface{}) (*Rows, error)

Query executes a query that returns rows, typically a SELECT. The args are for any placeholder parameters in the query.

▹ Example

▹ Example (MultipleResultSets)
func (*DB) QueryContext

func (db *DB) QueryContext(ctx context.Context, query string, args ...interface{}) (*Rows, error)

QueryContext executes a query that returns rows, typically a SELECT. The args are for any placeholder parameters in the query.
func (*DB) QueryRow

func (db *DB) QueryRow(query string, args ...interface{}) *Row

QueryRow executes a query that is expected to return at most one row. QueryRow always returns a non-nil value. Errors are deferred until Row's Scan method is called.

▹ Example
func (*DB) QueryRowContext

func (db *DB) QueryRowContext(ctx context.Context, query string, args ...interface{}) *Row

QueryRowContext executes a query that is expected to return at most one row. QueryRowContext always returns a non-nil value. Errors are deferred until Row's Scan method is called.
func (*DB) SetConnMaxLifetime

func (db *DB) SetConnMaxLifetime(d time.Duration)

SetConnMaxLifetime sets the maximum amount of time a connection may be reused.

Expired connections may be closed lazily before reuse.

If d <= 0, connections are reused forever.
func (*DB) SetMaxIdleConns

func (db *DB) SetMaxIdleConns(n int)

SetMaxIdleConns sets the maximum number of connections in the idle connection pool.

If MaxOpenConns is greater than 0 but less than the new MaxIdleConns then the new MaxIdleConns will be reduced to match the MaxOpenConns limit

If n <= 0, no idle connections are retained.
func (*DB) SetMaxOpenConns

func (db *DB) SetMaxOpenConns(n int)

SetMaxOpenConns sets the maximum number of open connections to the database.

If MaxIdleConns is greater than 0 and the new MaxOpenConns is less than MaxIdleConns, then MaxIdleConns will be reduced to match the new MaxOpenConns limit

If n <= 0, then there is no limit on the number of open connections. The default is 0 (unlimited).
func (*DB) Stats

func (db *DB) Stats() DBStats

Stats returns database statistics.
type DBStats

DBStats contains database statistics.

type DBStats struct {
        // OpenConnections is the number of open connections to the database.
        OpenConnections int
}

type IsolationLevel

IsolationLevel is the transaction isolation level used in TxOptions.

type IsolationLevel int

Various isolation levels that drivers may support in BeginTx. If a driver does not support a given isolation level an error may be returned.

See https://en.wikipedia.org/wiki/Isolation_(database_systems)#Isolation_levels.

const (
        LevelDefault IsolationLevel = iota
        LevelReadUncommitted
        LevelReadCommitted
        LevelWriteCommitted
        LevelRepeatableRead
        LevelSnapshot
        LevelSerializable
        LevelLinearizable
)

type NamedArg

A NamedArg is a named argument. NamedArg values may be used as arguments to Query or Exec and bind to the corresponding named parameter in the SQL statement.

For a more concise way to create NamedArg values, see the Named function.

type NamedArg struct {

        // Name is the name of the parameter placeholder.
        //
        // If empty, the ordinal position in the argument list will be
        // used.
        //
        // Name must omit any symbol prefix.
        Name string

        // Value is the value of the parameter.
        // It may be assigned the same value types as the query
        // arguments.
        Value interface{}
        // contains filtered or unexported fields
}

func Named

func Named(name string, value interface{}) NamedArg

Named provides a more concise way to create NamedArg values.

Example usage:

db.ExecContext(ctx, `
    delete from Invoice
    where
        TimeCreated < @end
        and TimeCreated >= @start;`,
    sql.Named("start", startTime),
    sql.Named("end", endTime),
)

type NullBool

NullBool represents a bool that may be null. NullBool implements the Scanner interface so it can be used as a scan destination, similar to NullString.

type NullBool struct {
        Bool  bool
        Valid bool // Valid is true if Bool is not NULL
}

func (*NullBool) Scan

func (n *NullBool) Scan(value interface{}) error

Scan implements the Scanner interface.
func (NullBool) Value

func (n NullBool) Value() (driver.Value, error)

Value implements the driver Valuer interface.
type NullFloat64

NullFloat64 represents a float64 that may be null. NullFloat64 implements the Scanner interface so it can be used as a scan destination, similar to NullString.

type NullFloat64 struct {
        Float64 float64
        Valid   bool // Valid is true if Float64 is not NULL
}

func (*NullFloat64) Scan

func (n *NullFloat64) Scan(value interface{}) error

Scan implements the Scanner interface.
func (NullFloat64) Value

func (n NullFloat64) Value() (driver.Value, error)

Value implements the driver Valuer interface.
type NullInt64

NullInt64 represents an int64 that may be null. NullInt64 implements the Scanner interface so it can be used as a scan destination, similar to NullString.

type NullInt64 struct {
        Int64 int64
        Valid bool // Valid is true if Int64 is not NULL
}

func (*NullInt64) Scan

func (n *NullInt64) Scan(value interface{}) error

Scan implements the Scanner interface.
func (NullInt64) Value

func (n NullInt64) Value() (driver.Value, error)

Value implements the driver Valuer interface.
type NullString

NullString represents a string that may be null. NullString implements the Scanner interface so it can be used as a scan destination:

var s NullString
err := db.QueryRow("SELECT name FROM foo WHERE id=?", id).Scan(&s)
...
if s.Valid {
   // use s.String
} else {
   // NULL value
}

type NullString struct {
        String string
        Valid  bool // Valid is true if String is not NULL
}

func (*NullString) Scan

func (ns *NullString) Scan(value interface{}) error

Scan implements the Scanner interface.
func (NullString) Value

func (ns NullString) Value() (driver.Value, error)

Value implements the driver Valuer interface.
type RawBytes

RawBytes is a byte slice that holds a reference to memory owned by the database itself. After a Scan into a RawBytes, the slice is only valid until the next call to Next, Scan, or Close.

type RawBytes []byte

type Result

A Result summarizes an executed SQL command.

type Result interface {
        // LastInsertId returns the integer generated by the database
        // in response to a command. Typically this will be from an
        // "auto increment" column when inserting a new row. Not all
        // databases support this feature, and the syntax of such
        // statements varies.
        LastInsertId() (int64, error)

        // RowsAffected returns the number of rows affected by an
        // update, insert, or delete. Not every database or database
        // driver may support this.
        RowsAffected() (int64, error)
}

type Row

Row is the result of calling QueryRow to select a single row.

type Row struct {
        // contains filtered or unexported fields
}

func (*Row) Scan

func (r *Row) Scan(dest ...interface{}) error

Scan copies the columns from the matched row into the values pointed at by dest. See the documentation on Rows.Scan for details. If more than one row matches the query, Scan uses the first row and discards the rest. If no row matches the query, Scan returns ErrNoRows.
type Rows

Rows is the result of a query. Its cursor starts before the first row of the result set. Use Next to advance through the rows:

rows, err := db.Query("SELECT ...")
...
defer rows.Close()
for rows.Next() {
    var id int
    var name string
    err = rows.Scan(&id, &name)
    ...
}
err = rows.Err() // get any error encountered during iteration
...

type Rows struct {
        // contains filtered or unexported fields
}

func (*Rows) Close

func (rs *Rows) Close() error

Close closes the Rows, preventing further enumeration. If Next is called and returns false and there are no further result sets, the Rows are closed automatically and it will suffice to check the result of Err. Close is idempotent and does not affect the result of Err.
func (*Rows) ColumnTypes

func (rs *Rows) ColumnTypes() ([]*ColumnType, error)

ColumnTypes returns column information such as column type, length, and nullable. Some information may not be available from some drivers.
func (*Rows) Columns

func (rs *Rows) Columns() ([]string, error)

Columns returns the column names. Columns returns an error if the rows are closed, or if the rows are from QueryRow and there was a deferred error.
func (*Rows) Err

func (rs *Rows) Err() error

Err returns the error, if any, that was encountered during iteration. Err may be called after an explicit or implicit Close.
func (*Rows) Next

func (rs *Rows) Next() bool

Next prepares the next result row for reading with the Scan method. It returns true on success, or false if there is no next result row or an error happened while preparing it. Err should be consulted to distinguish between the two cases.

Every call to Scan, even the first one, must be preceded by a call to Next.
func (*Rows) NextResultSet

func (rs *Rows) NextResultSet() bool

NextResultSet prepares the next result set for reading. It returns true if there is further result sets, or false if there is no further result set or if there is an error advancing to it. The Err method should be consulted to distinguish between the two cases.

After calling NextResultSet, the Next method should always be called before scanning. If there are further result sets they may not have rows in the result set.
func (*Rows) Scan

func (rs *Rows) Scan(dest ...interface{}) error

Scan copies the columns in the current row into the values pointed at by dest. The number of values in dest must be the same as the number of columns in Rows.

Scan converts columns read from the database into the following common Go types and special types provided by the sql package:

*string
*[]byte
*int, *int8, *int16, *int32, *int64
*uint, *uint8, *uint16, *uint32, *uint64
*bool
*float32, *float64
*interface{}
*RawBytes
any type implementing Scanner (see Scanner docs)

In the most simple case, if the type of the value from the source column is an integer, bool or string type T and dest is of type *T, Scan simply assigns the value through the pointer.

Scan also converts between string and numeric types, as long as no information would be lost. While Scan stringifies all numbers scanned from numeric database columns into *string, scans into numeric types are checked for overflow. For example, a float64 with value 300 or a string with value "300" can scan into a uint16, but not into a uint8, though float64(255) or "255" can scan into a uint8. One exception is that scans of some float64 numbers to strings may lose information when stringifying. In general, scan floating point columns into *float64.

If a dest argument has type *[]byte, Scan saves in that argument a copy of the corresponding data. The copy is owned by the caller and can be modified and held indefinitely. The copy can be avoided by using an argument of type *RawBytes instead; see the documentation for RawBytes for restrictions on its use.

If an argument has type *interface{}, Scan copies the value provided by the underlying driver without conversion. When scanning from a source value of type []byte to *interface{}, a copy of the slice is made and the caller owns the result.

Source values of type time.Time may be scanned into values of type *time.Time, *interface{}, *string, or *[]byte. When converting to the latter two, time.Format3339Nano is used.

Source values of type bool may be scanned into types *bool, *interface{}, *string, *[]byte, or *RawBytes.

For scanning into *bool, the source may be true, false, 1, 0, or string inputs parseable by strconv.ParseBool.
type Scanner

Scanner is an interface used by Scan.

type Scanner interface {
        // Scan assigns a value from a database driver.
        //
        // The src value will be of one of the following types:
        //
        //    int64
        //    float64
        //    bool
        //    []byte
        //    string
        //    time.Time
        //    nil - for NULL values
        //
        // An error should be returned if the value cannot be stored
        // without loss of information.
        Scan(src interface{}) error
}

type Stmt

Stmt is a prepared statement. A Stmt is safe for concurrent use by multiple goroutines.

type Stmt struct {
        // contains filtered or unexported fields
}

func (*Stmt) Close

func (s *Stmt) Close() error

Close closes the statement.
func (*Stmt) Exec

func (s *Stmt) Exec(args ...interface{}) (Result, error)

Exec executes a prepared statement with the given arguments and returns a Result summarizing the effect of the statement.
func (*Stmt) ExecContext

func (s *Stmt) ExecContext(ctx context.Context, args ...interface{}) (Result, error)

ExecContext executes a prepared statement with the given arguments and returns a Result summarizing the effect of the statement.
func (*Stmt) Query

func (s *Stmt) Query(args ...interface{}) (*Rows, error)

Query executes a prepared query statement with the given arguments and returns the query results as a *Rows.
func (*Stmt) QueryContext

func (s *Stmt) QueryContext(ctx context.Context, args ...interface{}) (*Rows, error)

QueryContext executes a prepared query statement with the given arguments and returns the query results as a *Rows.
func (*Stmt) QueryRow

func (s *Stmt) QueryRow(args ...interface{}) *Row

QueryRow executes a prepared query statement with the given arguments. If an error occurs during the execution of the statement, that error will be returned by a call to Scan on the returned *Row, which is always non-nil. If the query selects no rows, the *Row's Scan will return ErrNoRows. Otherwise, the *Row's Scan scans the first selected row and discards the rest.

Example usage:

var name string
err := nameByUseridStmt.QueryRow(id).Scan(&name)

func (*Stmt) QueryRowContext

func (s *Stmt) QueryRowContext(ctx context.Context, args ...interface{}) *Row

QueryRowContext executes a prepared query statement with the given arguments. If an error occurs during the execution of the statement, that error will be returned by a call to Scan on the returned *Row, which is always non-nil. If the query selects no rows, the *Row's Scan will return ErrNoRows. Otherwise, the *Row's Scan scans the first selected row and discards the rest.

Example usage:

var name string
err := nameByUseridStmt.QueryRowContext(ctx, id).Scan(&name)

type Tx

Tx is an in-progress database transaction.

A transaction must end with a call to Commit or Rollback.

After a call to Commit or Rollback, all operations on the transaction fail with ErrTxDone.

The statements prepared for a transaction by calling the transaction's Prepare or Stmt methods are closed by the call to Commit or Rollback.

type Tx struct {
        // contains filtered or unexported fields
}

func (*Tx) Commit

func (tx *Tx) Commit() error

Commit commits the transaction.
func (*Tx) Exec

func (tx *Tx) Exec(query string, args ...interface{}) (Result, error)

Exec executes a query that doesn't return rows. For example: an INSERT and UPDATE.
func (*Tx) ExecContext

func (tx *Tx) ExecContext(ctx context.Context, query string, args ...interface{}) (Result, error)

ExecContext executes a query that doesn't return rows. For example: an INSERT and UPDATE.
func (*Tx) Prepare

func (tx *Tx) Prepare(query string) (*Stmt, error)

Prepare creates a prepared statement for use within a transaction.

The returned statement operates within the transaction and can no longer be used once the transaction has been committed or rolled back.

To use an existing prepared statement on this transaction, see Tx.Stmt.
func (*Tx) PrepareContext

func (tx *Tx) PrepareContext(ctx context.Context, query string) (*Stmt, error)

Prepare creates a prepared statement for use within a transaction.

The returned statement operates within the transaction and will be closed when the transaction has been committed or rolled back.

To use an existing prepared statement on this transaction, see Tx.Stmt.

The provided context will be used for the preparation of the context, not for the execution of the returned statement. The returned statement will run in the transaction context.
func (*Tx) Query

func (tx *Tx) Query(query string, args ...interface{}) (*Rows, error)

Query executes a query that returns rows, typically a SELECT.
func (*Tx) QueryContext

func (tx *Tx) QueryContext(ctx context.Context, query string, args ...interface{}) (*Rows, error)

QueryContext executes a query that returns rows, typically a SELECT.
func (*Tx) QueryRow

func (tx *Tx) QueryRow(query string, args ...interface{}) *Row

QueryRow executes a query that is expected to return at most one row. QueryRow always returns a non-nil value. Errors are deferred until Row's Scan method is called.
func (*Tx) QueryRowContext

func (tx *Tx) QueryRowContext(ctx context.Context, query string, args ...interface{}) *Row

QueryRowContext executes a query that is expected to return at most one row. QueryRowContext always returns a non-nil value. Errors are deferred until Row's Scan method is called.
func (*Tx) Rollback

func (tx *Tx) Rollback() error

Rollback aborts the transaction.
func (*Tx) Stmt

func (tx *Tx) Stmt(stmt *Stmt) *Stmt

Stmt returns a transaction-specific prepared statement from an existing statement.

Example:

updateMoney, err := db.Prepare("UPDATE balance SET money=money+? WHERE id=?")
...
tx, err := db.Begin()
...
res, err := tx.Stmt(updateMoney).Exec(123.45, 98293203)

The returned statement operates within the transaction and will be closed when the transaction has been committed or rolled back.
func (*Tx) StmtContext

func (tx *Tx) StmtContext(ctx context.Context, stmt *Stmt) *Stmt

StmtContext returns a transaction-specific prepared statement from an existing statement.

Example:

updateMoney, err := db.Prepare("UPDATE balance SET money=money+? WHERE id=?")
...
tx, err := db.Begin()
...
res, err := tx.StmtContext(ctx, updateMoney).Exec(123.45, 98293203)

The returned statement operates within the transaction and will be closed when the transaction has been committed or rolled back.
type TxOptions

TxOptions holds the transaction options to be used in DB.BeginTx.

type TxOptions struct {
        // Isolation is the transaction isolation level.
        // If zero, the driver or database's default level is used.
        Isolation IsolationLevel
        ReadOnly  bool
}
