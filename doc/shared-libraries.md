Shared Libraries
================

## Crowncoinconsensus

The purpose of this library is to make the verification functionality that is critical to Crowncoin's consensus available to other applications, e.g. to language bindings.

### API

The interface is defined in the C header `Crowncoinconsensus.h` located in  `src/script/Crowncoinconsensus.h`.

#### Version

`Crowncoinconsensus_version` returns an `unsigned int` with the API version *(currently at an experimental `0`)*.

#### Script Validation

`Crowncoinconsensus_verify_script` returns an `int` with the status of the verification. It will be `1` if the input script correctly spends the previous output `scriptPubKey`.

##### Parameters
- `const unsigned char *scriptPubKey` - The previous output script that encumbers spending.
- `unsigned int scriptPubKeyLen` - The number of bytes for the `scriptPubKey`.
- `const unsigned char *txTo` - The transaction with the input that is spending the previous output.
- `unsigned int txToLen` - The number of bytes for the `txTo`.
- `unsigned int nIn` - The index of the input in `txTo` that spends the `scriptPubKey`.
- `unsigned int flags` - The script validation flags *(see below)*.
- `Crowncoinconsensus_error* err` - Will have the error/success code for the operation *(see below)*.

##### Script Flags
- `Crowncoinconsensus_SCRIPT_FLAGS_VERIFY_NONE`
- `Crowncoinconsensus_SCRIPT_FLAGS_VERIFY_P2SH` - Evaluate P2SH ([BIP16](https://github.com/Crowncoin/bips/blob/master/bip-0016.mediawiki)) subscripts
- `Crowncoinconsensus_SCRIPT_FLAGS_VERIFY_DERSIG` - Enforce strict DER ([BIP66](https://github.com/Crowncoin/bips/blob/master/bip-0066.mediawiki)) compliance
- `Crowncoinconsensus_SCRIPT_FLAGS_VERIFY_NULLDUMMY` - Enforce NULLDUMMY ([BIP147](https://github.com/Crowncoin/bips/blob/master/bip-0147.mediawiki))
- `Crowncoinconsensus_SCRIPT_FLAGS_VERIFY_CHECKLOCKTIMEVERIFY` - Enable CHECKLOCKTIMEVERIFY ([BIP65](https://github.com/Crowncoin/bips/blob/master/bip-0065.mediawiki))
- `Crowncoinconsensus_SCRIPT_FLAGS_VERIFY_CHECKSEQUENCEVERIFY` - Enable CHECKSEQUENCEVERIFY ([BIP112](https://github.com/Crowncoin/bips/blob/master/bip-0112.mediawiki))
- `Crowncoinconsensus_SCRIPT_FLAGS_VERIFY_WITNESS` - Enable WITNESS ([BIP141](https://github.com/Crowncoin/bips/blob/master/bip-0141.mediawiki))

##### Errors
- `Crowncoinconsensus_ERR_OK` - No errors with input parameters *(see the return value of `Crowncoinconsensus_verify_script` for the verification status)*
- `Crowncoinconsensus_ERR_TX_INDEX` - An invalid index for `txTo`
- `Crowncoinconsensus_ERR_TX_SIZE_MISMATCH` - `txToLen` did not match with the size of `txTo`
- `Crowncoinconsensus_ERR_DESERIALIZE` - An error deserializing `txTo`
- `Crowncoinconsensus_ERR_AMOUNT_REQUIRED` - Input amount is required if WITNESS is used

### Example Implementations
- [NCrowncoin](https://github.com/NicolasDorier/NCrowncoin/blob/master/NCrowncoin/Script.cs#L814) (.NET Bindings)
- [node-libCrowncoinconsensus](https://github.com/bitpay/node-libCrowncoinconsensus) (Node.js Bindings)
- [java-libCrowncoinconsensus](https://github.com/dexX7/java-libCrowncoinconsensus) (Java Bindings)
- [Crowncoinconsensus-php](https://github.com/Bit-Wasp/Crowncoinconsensus-php) (PHP Bindings)
