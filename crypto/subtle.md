
 Package subtle

    import "crypto/subtle"

    Overview
    Index

Overview ▾

Package subtle implements functions that are often useful in cryptographic code but require careful thought to use correctly.
Index ▾

    func ConstantTimeByteEq(x, y uint8) int
    func ConstantTimeCompare(x, y []byte) int
    func ConstantTimeCopy(v int, x, y []byte)
    func ConstantTimeEq(x, y int32) int
    func ConstantTimeLessOrEq(x, y int) int
    func ConstantTimeSelect(v, x, y int) int

Package files

constant_time.go
func ConstantTimeByteEq

func ConstantTimeByteEq(x, y uint8) int

ConstantTimeByteEq returns 1 if x == y and 0 otherwise.
func ConstantTimeCompare

func ConstantTimeCompare(x, y []byte) int

ConstantTimeCompare returns 1 if and only if the two slices, x and y, have equal contents. The time taken is a function of the length of the slices and is independent of the contents.
func ConstantTimeCopy

func ConstantTimeCopy(v int, x, y []byte)

ConstantTimeCopy copies the contents of y into x (a slice of equal length) if v == 1. If v == 0, x is left unchanged. Its behavior is undefined if v takes any other value.
func ConstantTimeEq

func ConstantTimeEq(x, y int32) int

ConstantTimeEq returns 1 if x == y and 0 otherwise.
func ConstantTimeLessOrEq

func ConstantTimeLessOrEq(x, y int) int

ConstantTimeLessOrEq returns 1 if x <= y and 0 otherwise. Its behavior is undefined if x or y are negative or > 2**31 - 1.
func ConstantTimeSelect

func ConstantTimeSelect(v, x, y int) int

ConstantTimeSelect returns x if v is 1 and y if v is 0. Its behavior is undefined if v takes any other value. 
