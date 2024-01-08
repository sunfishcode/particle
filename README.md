the existence of a text format for wasm values called wave implies the existence
of a corresponding binary format for wasm values called particle

There are many things that *can* be optimized for in a binary format. Here's
some preliminary brainstorming about a hypothetical point in the design space:

 - As with Wave, values themselves don't encode their full types. This saves
   size, but means that type information must be negotiated at a higher level.

 - There's no factoring of redundancies between expressions. Combine with a
   general-purpose compressor (eg. gzip or others) when this is important.
   
 - Encode strings as UTF-8, preceded by a length in code units, so that consumers can
   allocate a buffer up front, or skip over the string value.

 - Don't try to implement non-coercive subtyping at the expression level. Higher-level
   formats and protocols should record the types that expressions are encoded with, and
   subtyping conversions should be performed by consumers recognizing that the encoded
   types differ from the destination types.

 - Encode integers as plain-binary rather than LEB128 or some other compressed form.
   This allows eg. list<u8> to be plain-binary. Encode floats as plain-binary too,
   though canonicalize NaNs (to sign = 0, quiet = 1, rest = 0).
