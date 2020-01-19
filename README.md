# ByteArray-node

[![npm version](https://img.shields.io/npm/v/bytearray-node?style=flat-square)](https://www.npmjs.com/package/bytearray-node)
[![Dependencies](https://img.shields.io/david/Zaseth/bytearray-node)](https://www.npmjs.com/package/bytearray-node?activeTab=dependencies)

A Node.js implementation of the Actionscript 3 ByteArray supporting AMF0/AMF3.

**XML, Vector.object and weak dictionary types are not supported.**

**Dictionaries are Maps and Vectors are typed arrays that you can only make fixed by using Object.seal.**

# Installation

`npm install bytearray-node`

# Usage

For extended usage, see [here](https://github.com/Zaseth/bytearray-node/tree/master/test)

```javascript
const ByteArray = require('bytearray-node')
const Endian = require('bytearray-node/enums/Endian')
const ObjectEncoding = require('bytearray-node/enums/ObjectEncoding')
const CompressionAlgorithm = require('bytearray-node/enums/CompressionAlgorithm')

const ba = new ByteArray()

//ba.endian = Endian.BIG_ENDIAN
//ba.endian = Endian.LITTLE_ENDIAN
//ba.endian = Endian.SYSTEM

//ba.objectEncoding = ObjectEncoding.AMF0
//ba.objectEncoding = ObjectEncoding.AMF3
//ba.objectEncoding = ObjectEncoding.DEFAULT

//CompressionAlgorithm.DEFLATE
//CompressionAlgorithm.LZMA
//CompressionAlgorithm.ZLIB

ba.writeByte(1)
ba.writeShort(5)

ba.position = 0

console.log(ba.readByte()) // 1
console.log(ba.readShort()) // 5
```

# AMF IExternalizable example

```javascript
const ByteArray = require('bytearray-node')
const IExternalizable = require('bytearray-node/enums/IExternalizable')

class Person extends IExternalizable {
  constructor(name) {
    this.name = name
  }

  writeExternal(ba) {
    ba.writeUTF(this.name)
  }

  readExternal(ba) {
    this.name = ba.readUTF()
  }
}

ByteArray.registerClassAlias('src.person', Person)

const ba = new ByteArray()
const person = new Person('Daan')

ba.writeObject(person)

ba.position = 0

console.log(ba.readObject()) // Person { name: 'Daan' }
```
