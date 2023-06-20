```go
yc.WithKeyID(v.GetString(KeyYdbSaKeyID)),
yc.WithIssuer(v.GetString(KeyYdbSaId)),  
yc.WithPrivateKeyFile(v.GetString(KeyYdbSaPrivateKeyFile)),
```

```go
func parsePrivateKey(raw []byte) (*rsa.PrivateKey, error) {  
block, _ := pem.Decode(raw)  
if block == nil {  
return nil, ErrKeyCannotBeParsed  
}  
key, err := x509.ParsePKCS1PrivateKey(block.Bytes)  
if err == nil {  
return key, err  
}  
  
x, err := x509.ParsePKCS8PrivateKey(block.Bytes)  
if err != nil {  
return nil, err  
}  
if key, ok := x.(*rsa.PrivateKey); ok {  
return key, nil  
}  
return nil, ErrKeyCannotBeParsed  
}
```