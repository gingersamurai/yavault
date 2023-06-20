```go
yc.WithKeyID(v.GetString(KeyYdbSaKeyID)),
yc.WithIssuer(v.GetString(KeyYdbSaId)),  
yc.WithPrivateKeyFile(v.GetString(KeyYdbSaPrivateKeyFile)),
```