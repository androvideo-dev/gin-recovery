# gin-recovery
The recovery of gin.


## Example
**Setup recovery**
```go
import (
	"github.com/gin-gonic/gin"
	"strings"
	"github.com/androvideo-dev/gin-recovery"
)

func main() {
	router := gin.Default()
	router.Use(recovery.Recovery(recoveryHandler))
}

func recoveryHandler(c *gin.Context, error *recovery.HttpError) {
	if c.Request.Header.Get("Content-Type") == "application/json" {
		c.AbortWithStatusJSON(error.Status, gin.H{
			"error": error.Message,
			"stack": strings.Split(string(recovery.Stack(4)), "\n"),
			"extra": error.Extra,
		})
	} else {
		// render html error page.
		c.HTML(error.Status, "error.html", gin.H{
			"title": "Error",
			"error": error,
		})
		c.Abort()
	}
}
```

**Panic**
```go
import (
	"github.com/androvideo-dev/gin-recovery"
)

func SomeHandler(c *gin.Context) {
	panic(recovery.HttpError{
		Status: 500,
		Message: "Some error.",
	})
	return
}
```
