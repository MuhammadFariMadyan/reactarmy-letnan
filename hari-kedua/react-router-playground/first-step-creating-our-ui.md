## First Step

Langkah pertama adalah membuat UI dari **TodoList **kita, kita akan menggunakan Reactstrap \(Bootstrap React\) untuk melakukan styling pada aplikasi **TodoList **kita, akan tetapi kalian bebas untuk berkreasi menggunakan Pure CSS atau Framework CSS di luar sana sesuai selera kalian.

![](/assets/Screenshot_7.png)  
kurang lebih tampilan UI **TodoList **kita akan seperti di atas, Kita membutuhkan **1 container** dengan **1 buah tag h1** dan **2 buah row** \(1 untuk List dan 1 lagi untuk inputan form\)

```
<div className="container text-center">
    <h1 className="mt-5">Mt Todo Lists</h1>
    <div className="row">
        <div className="col-md-6 mx-auto">
        //Your Lists goes here
        </div>
    </div>
    <div className="row mt-3">
        <div className="col-md-6 mx-auto">
            //Your Input goes here
        </div>
    </div>
</div
```

**List**

```jsx
import { ListGroup, ListGroupItem } from 'reactstrap';

//...
<ListGroup>
    <ListGroupItem>
        <div className="d-flex">
            <div className="p-2">Todo List 1</div>
            <div className="ml-auto p-2">
                <button
                    className="btn btn-danger"
                    onClick={() => this.onRemove(index)}>
                    X
                 </button>
            </div>
        </div>
    </ListGroupItem>
</ListGroup>
//...
```

**Input**

```
//...
<div className="col-md-6 mx-auto">
    <div className="d-flex">
       <input
           className="form-control"
           type="text"
           placeholder="insert todo list"
           ref={el => (this.input = el)}
        />
        <button
            className="btn btn-outline-primary"
            onClick={this.addTodo}>
            Add Todo List
         </button>
     </div>
</div>
//...
```

And Voila UI kita sudah selesai Code Lengkap ada [di sini](https://gist.github.com/MirzaChilman/ef3014665acd1fb8eed470f713f7073d)

### Filling Our Function

Mari kita buat Pseudocode sederhana untuk aplikasi kita

1. Aplikasi mempunyai 1 inputan dan 1 tombol
2. Ketika tombol _**insert **_di klik maka listakan muncul
3. Setiap list akan menghasilkan text dan tombol
4. Ketika tombol _**delete**_ di list di klik maka list akan menghilang
5. Setiap list yang tersimpan akan di simpan di _**Local Storage**_

Berdasarkan Pseudocode sederhana di atas maka kita membutuhkan 2 buah fungsi yaitu **onAddHandler** dan **onRemoveHandler**



