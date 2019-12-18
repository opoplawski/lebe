# LEBE
Tiny, dead simple, high performance endianness conversions

# Purpose
This crate has exactly two purposes:
  1. Simple conversion between slices of primitives and byte arrays without unsafe code
  2. Simple and fast conversion from one endianness to the other one

This simplifies writing binary data to files.


Also, it's tiny! The source code is literally 250 lines of Rust, 
when counting neither documentation, tests nor benchmarks.

# Usage

Convert slices in-place.
```rust
    use lebe::Endian;
    
    fn main(){
        let mut numbers: &[i32] = &[ 32, 102, 420, 594 ];
        numbers.make_le();
    }
```

Write slices.
```rust
    use lebe::io::WriteEndian;
    use std::io::Write;
    
    fn main(){
        let numbers: &[i32] = &[ 32, 102, 420, 594 ];
        
        let mut output_bytes: Vec<u8> = Vec::new();
        output_bytes.write_le(numbers).unwrap();
    }
```

Read numbers.
```rust
    use lebe::io::ReadEndian;
    use std::io::Read;
    
    fn main(){
        let mut input_bytes: &[u8] = &[ 3, 244 ];
        let number: u16 = input_bytes.read_le().unwrap();
    }
```

Read slices.
```rust
    use lebe::io::ReadEndian;
    use std::io::Read;
    
    fn main(){
        let mut numbers: &[i32] = &[ 0; 2 ];
        
        let mut input_bytes: &[u8] = &[ 0, 3, 244, 1, 0, 3, 244, 1 ];
        input_bytes.read_le_into(&mut numbers).unwrap();
    }
```


# Why not use [byteorder](https://crates.io/crates/byteorder)?
This crate supports batch-writing slices with native speed 
where the os has the matching endianness. Writing slices in `byteorder` 
must be done manually, and may be slower than expected. 
This crate does provide u8 and i8 slice operations for completeness.
Also, the API of this crate looks simpler.

# Why not use [endianness](https://crates.io/crates/endianness)?
This crate has no runtime costs, just as `byteorder`.

# Why not use this crate?
This crate requires a fairly up-to-date rust version, 
which not all projects can support.


# Fun Facts
LEBE is made up from 'le' for little endian and 'be' for big endian.
If you say that word using english pronounciation, 
a german might think you said the german word for 'love'.
