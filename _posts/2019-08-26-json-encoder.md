---
layout: post
author: jayahariv
title: apple json encoder
---
# JSON Encoder

#### JSONKeyedDecodingContainer
- decoder: `codingPath` is appended whenever decodes, and removes at the end.
- container: Values are accessed from the container

##### Error handled
- DecodingError.keyNotFound
- DecodingError.valueNotFound
- DecodingError.typeMismatch

##### Misc
- NestedKeyedContainer: Get the dictionary for the key, and pass the dictionary(container) & decoder to the the new decodingContainer. return the KeyedDecodingContainer(decodingKeyedContainer)

- NestedUnKeyedContainer: Get the array for the key, and pass the array & decoder to the new decodingUnKeyedContainer. Return the decodingUnKeyedContainer
