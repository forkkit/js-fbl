
## Schema

`Flexible Byte Layout` is an advanced layout for representing binary data.

It is flexible enough to support very small and very large (multi-block) binary data.

```sh
type Lengths [Int]
type ByteUnionList [&BytesUnion]

type NestedByteList struct {
  lengths Lengths
  parts ByteUnionList
  algo optional String
}

type BytesUnion union {
  | Bytes bytes
  | NestedByteList map
} representation kinded

type FlexibleByteLayout struct {
  bytes BytesUnion
  size Int
}
```

`FlexibleByteLayout` uses a potentially recursive union type. This allows you to build very large nested
dags via NestedByteList that can themselves contain additional NestedByteLists, links to BytesUnions.

An implementation must define a custom function for reading ranges of binary
data but once implemented, you can read data regardless of the layout algorithm used.

Since readers only need to concern themselves with implementing the read method, they **do not**
need to understand the algorithms used to generate the layouts. This gives a lot of flexibility
in the future to define new layout algorithms as necessary without needing to worry about
updating prior impelementations.
