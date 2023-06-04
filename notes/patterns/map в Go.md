
```go
type maptype struct {  
	typ _type  
	key *_type  
	elem *_type  
	bucket *_type // internal type representing a hash bucket  
	// function for hashing keys (ptr to key, seed) -> hash  
	hasher func(unsafe.Pointer, uintptr) uintptr  
	keysize uint8 // size of key slotelemsize uint8 // size of elem slotbucketsize uint16 // size of bucket  
	flags uint32  
}

type _type struct {  
	size uintptr  
	ptrdata uintptr // size of memory prefix holding all pointershash uint32  
	tflag tflag  
	align uint8  
	fieldAlign uint8  
	kind uint8  
	// function for comparing objects of this type  
	// (ptr to object A, ptr to object B) -> ==?  
	equal func(unsafe.Pointer, unsafe.Pointer) bool  
	// gcdata stores the GC type data for the garbage collector.// If the KindGCProg bit is set in kind, gcdata is a GC program.// Otherwise it is a ptrmask bitmap. See mbitmap.go for details.  
	gcdata *byte  
	str nameOff  
	ptrToThis typeOff  
}
```

![](Pasted%20image%2020230525163029.png)
![](Pasted%20image%2020230525163208.png)
![](Pasted%20image%2020230525165438.png)

```go
// A header for a Go map.  
type hmap struct {  
	// Note: the format of the hmap is also encoded in cmd/compile/internal/reflectdata/reflect.go.// Make sure this stays in sync with the compiler's definition.  
	count int // # live cells == size of map. Must be first (used by len() builtin)  
	flags uint8  
	B uint8 // log_2 of # of buckets (can hold up to loadFactor * 2^B items)  
	noverflow uint16 // approximate number of overflow buckets; see incrnoverflow for detailshash0 uint32 // hash seed  
	  
	buckets unsafe.Pointer // array of 2^B Buckets. may be nil if count==0.  
	oldbuckets unsafe.Pointer // previous bucket array of half the size, non-nil only when growing  
	nevacuate uintptr // progress counter for evacuation (buckets less than this have been evacuated)  
	  
	extra *mapextra // optional fields  
}
```
