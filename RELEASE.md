## Release Process

Binaries for the C++ binding file (which includes libbitcoind statically linked in) are distributed for convenience. The binary binding file `bitcoind.node` is signed and published to S3 for later download and installation. Source files can also be built if binaries are not desired.

### How to Verify Signatures

```
cd build/Release
gpg --verify bitcoind.node.sig bitcoind.node
```

To verify signatures, use the following PGP keys:

- @braydonf: https://pgp.mit.edu/pks/lookup?op=get&search=0x9BBF07CAC07A276D
- @kleetus: https://pgp.mit.edu/pks/lookup?op=get&search=0x33195D27EF6BDB7F
- @pnagurny: https://pgp.mit.edu/pks/lookup?op=get&search=0x0909B33F0AA53013

### How to Release

Ensure you've followed the instructions in the README.md for building the project from source. You will be using node-gyp to build the C++ bindings. A script will then upload the bindings to S3 for later use. You will also need credentials for BitPay's bitcore-node S3 bucket and be listed as an author for the bitcore-node's npm module.

- Create a file `.bitcore-node-upload.json` in your home directory
- The format of this file should be:

```json
{
  "region": "us-east-1",
  "accessKeyId": "xxx",
  "secretAccessKey": "yyy"
}
```

To make a release, bump the version of the package.json:

```bash
git checkout master
git pull upstream master
git commit -a -m "Bump package version to <version>"
npm install
npm run package
npm run upload
npm publish
```

And then update the version of the package.json for development (e.g. "0.3.2-dev"):

```bash
git commit -a -m "Bump development version to <version>"
git push upstream master
```