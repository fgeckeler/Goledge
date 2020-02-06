# Grouping data

## go's data grouping types

- slice: `x := []int {1,2,3}`
- loop through data: `for i, v := range x{}` // give me index and value
- slice a slice: `x[0:2]` // returns index 0-1 ; not including index 2
- append to slice: `y := []int {6,7,8}; x = append(x, y...)` // takes 2 slices and appends b to a
- delete from slice: `append(x[:2], y[4:]...)`
- slice sits on array. Possbile to use `make` to set max capacity for underlying array // saves time for compiler
- multi dimensional slices: `[][] string{slice a, slice b}`
- Maps: `m := map[string]int {"a": 1}`
- check for k,v exists in map and get value of key: `if v, ok := m["key"]; ok {do sth.}`
- add element to map `` // just new key, value on the same object
- loop over map: `for k, v := range m {do sth.}`
- delete: `delete(map, "key")`

## Structs

- nested structs: inner types are promoted to outer types. On name collision use namespaces. Unqualified type name (here person in coolDude struct) acts as field name.

```golang
type person struct {
    first string
    last string
    next string
}
type coolDude struct {
    person
    next string
}

dude := coolDude{ person: {first: "asd", last: "bsd", next: "old"}, next: "wad"}
fmt.Println(dude.person.next)
fmt.Println(dude.next)
```

- anonymous structs: instead of defining a strcut explicitly define value of a var as a struct with following values:

```golang
func main(){
    person:= struct {
        first string
        last string
        next string
    }{first: "asd", last: "basd"}
    fmt.Println(person)
}
```
