# API Review 10/10/2023

## More granular X.509 certificate loader

**NeedsWork** | [#runtime/91763](https://github.com/dotnet/runtime/issues/91763#issuecomment-1756097064) | [Video](https://www.youtube.com/watch?v=5HgJIi2dqjs&t=0h0m0s)

API Review comments:

Pkcs12LoaderLimits:
* Add MakeReadOnly, IsReadOnly and a copy constructor
* Remove AllowMultipleEncryptedSegments
* IgnoreEncryptedSegments => IgnoreEncryptedItems if it's all items, IgnoreEncryptedAuthSafes if it's auth safes.
* MaxCerts => MaxCertificates

X509CertificateLoader:
* Remove SerializedCert, SerializedStore, AuthenticodeSigner, Pkcs7PrimarySigner, and Pkcs7EmbeddedCertificates
* Rename LoadX509Certificate to LoadCertificate
* LoadPkcs12 => LoadPkcs12Collection
* LoadPkcs12SingleCertificate => LoadPkcs12

To be continued...
