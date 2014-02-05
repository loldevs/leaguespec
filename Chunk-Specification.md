# Header

```
| 00          04 |
|----------------|
| AA BB BB BB BB |
```

* **AA** Seems to be *03* or *04* all the time
* **BB BB BB BB** Timestamp in seconds as 32bit float in little endian byte order (IEEE-754)

The time will be all zero for champion select / loading chunks. 
