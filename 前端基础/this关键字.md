## this

```
var name = 'mt'
var obj = {
    name : 'suyi',
    getName: function(){
        return this.name
    },
    gName: function(){
        function inner(){
            return this.name
        }        
        inner()
    },
    getClosure: function(){
        var y = 0
        return function(){
          console.log(this.name, y++)
        }
    }    
}

var func = obj.getName
var clus = obj.getClosure()


obj.getName() // 'suyi'

(obj.getName=obj.getName)() // 'mt'

func() // 'mt'

(obj.getName=obj.getName())() // Error, not function

clus() // 'mt', 0

(obj.getClosure=obj.getClosure())()   // 'mt', 0
((obj.getClosure=obj.getClosure)())() // 'mt', 0
```



