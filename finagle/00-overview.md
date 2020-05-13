## finagle
- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 20120-03-17


#### Futures

```
encapsulate and compose concurrent operations
it no problem to maintain millions of outstanding oncurrent operations when they are represented by futures
decouple finagle from operating system and runtime thread schedulers
```

- don't perform blocking operatings in  a futures
- must yield control via flatMap
- 
