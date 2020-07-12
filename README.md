# App-REST-API

Курс пройден при поддержке канала
https://www.youtube.com/watch?v=lzQIhjElV_g&t=1426s

Методы PUT, GET, DELETE, POST 

#### Серверная часть
Используемые технологии: 
- NodeJS
- Express
- uuid
- nodemon

GET
```js
app.get('/api/contacts', (req, res) => {
    setTimeout(() => {
        res.status(200).json(CONTACTS)
    }, 300)
})
```
POST
```js
app.post('/api/contacts', (req, res)=>{
    console.log(req.body);
    const contact = {...req.body, id: v4(), marked:  false}
    CONTACTS.push(contact)
    res.status(201).json(contact)
})
```
DELETE
```js
app.delete('/api/contacts/:id', (req, res) => {
    CONTACTS = CONTACTS.filter(c => c.id !== req.params.id)
    res.status(200).json({message: 'Контакт был удален'})
})
```
 PUT
```js
app.put('/api/contacts/:id', (req, res) => {
    const idx = CONTACTS.findIndex(c => c.id === req.params.id)
    CONTACTS[idx] = req.body
    res.json(CONTACTS[idx])
})
```



#### Клиентская часть
Используемые технологии: 
- Bootstarp
- Vue

GET
``` js
async function request(url, method = 'GET', data = null) {
    try {
        const headers = {}
        let body
        if (data) {
            headers['Content-Type'] = 'application/json'
            body = JSON.stringify(data)
        }
        const response = await fetch(url, {
            method: method,
            headers: headers,
            body: body
        })
        return await response.json()
    } catch (e) {
        console.warn('Error', e.message)
    }
}
```

POST
``` js 
async createContact() {
    const { ...contact } = this.form
    const newContact = await request('/api/contacts', 'POST', contact)
    this.contacts.push(newContact)
    this.form.name = this.form.value = ''
}
``` 

DELETE
``` js
async removeContact(id) {
    await request(`/api/contacts/${id}`, 'DELETE')
    this.contacts = this.contacts.filter(c => c.id !== id)
}
``` 

PUT
``` js 
async markContact(id) {
    const contact = this.contacts.find(c => c.id === id)
    const update = await request(`/api/contacts/${id}`, 'PUT', {
        ...contact,
        marked: true
    })
    contact.marked = update.marked
}
```



