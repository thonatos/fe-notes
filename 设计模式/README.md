设计模式

# factory

```
function createBook(name, url){
    let book = {}
    book.name = name
    book.url = url
    return book
}

const feNotes = createBook('fe-notes', 'http://github.com/thonatos/fe-notes')
const beNotes = createBook('be-notes', 'http://github.com/thonatos/be-notes')
```

# constructure

```
function Book(name, url){
    this.name = name
    this.url = url
}

const feNotes = new Book('fe-notes', 'http://github.com/thonatos/fe-notes')
const beNotes = Book('be-notes', 'http://github.com/thonatos/be-notes')
```



