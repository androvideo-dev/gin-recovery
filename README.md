# gin-recovery
The recovery of gin.


```go
import recovery

router.Use(recovery.Recovery(recoveryHandler))

func recoveryHandler(c *gin.Context, error *recovery.HttpError) {
	if c.Request.Header.Get("Content-Type") == "application/json" {
		c.AbortWithStatusJSON(error.Status, gin.H{
			"error": error.Message,
			"stack": strings.Split(string(errors.Stack(4)), "\n"),
			"extra": error.Extra,
		})
	} else {
		// render html error page.
		c.HTML(500, "error.html", gin.H{
			"title": "Error",
			"err": error,
		})
		c.Abort()
	}
}
```
