## First Step

Langkah pertama adalah membuat UI dari **TodoList**kita, kita akan menggunakan Reactstrap \(Bootstrap React\) untuk melakukan styling pada aplikasi **TodoList**kita, akan tetapi kalian bebas untuk berkreasi menggunakan Pure CSS atau Framework CSS di luar sana sesuai selera kalian.

![](/assets/Screenshot_7.png)  
kurang lebih tampilan UI **TodoList** kita akan seperti di atas, Kita membutuhkan **1 container** dengan **1 buah tag h1** dan **2 buah row** \(1 untuk List dan 1 lagi untuk inputan form\)

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

```jsx
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
2. Ketika tombol \_**insert **\_di klik maka listakan muncul
3. Setiap list akan menghasilkan text dan tombol
4. Ketika tombol _**delete**_ di list di klik maka list akan menghilang
5. Setiap list yang tersimpan akan di simpan di _**Local Storage**_

Berdasarkan Pseudocode sederhana di atas maka kita membutuhkan 2 buah fungsi yaitu **onAddHandler** dan **onRemoveHandler** maka dari itu hal pertama yang akan kita lakukan adalah membuat _constructor _

#### onAddHandler

```jsx
export default class App extends Component {
    constructor () {
        super();
        this.state = {
            query:'',
            item : JSON.parse(localStorage.getItem('localData')),
        }
    }
    //.....
}
```

Mungkin kalian melihat hal yang tidak biasa `JSON.parse(localStorage.getItem('localData')),` _syntax _itu diperlukan untuk memparsing data yang berada di _**Local Storage**_ yang tersimpan dengan nama _**localData,**_tanpa menggunakan hal tersebut maka setiap kali kita me-reload atau me-restart aplikasi kita maka data akan kembali kosong.

Mari melanjutkan dengan fungsi pertama kita yaitu **onAddHandler**

```jsx
onAddHandler = () => {
    let inputValue = this.input.value;
    
    if (localStorage.getItem('localData) === null){
        let item = [];
        item.push(inputValue);
        localStorage.setItem('localData',JSON.stringify(item))
    }else {
        let item = JSON.parse(localStorage.getItem('localData));
        item.push(inputValue);
        localStorage.setItem('localData', JSON.stringify(item))
    }
    //...
}
```

mari kita bahas sepenggal code di atas, kita membuat sebuah variabel baru bernama **inputValue** variabel tersebut berguna untuk menyimpan value \(nilai\) dari inputan, jadi ketika kita mengetik sesuatu, maka variabale **inputValue** akan menyimpan hasilnya. Kemudian untuk **IF statement** pertama kita ingin mengecek apakah **Local Storage localData** bernilai kosong yang berarti belum bernilai, jika memang belum bernilai dan masih kosong maka variabel **item** akan bernilai sebuah array kosong, dan nilai dari inputan yang sudah kita ketik akan di **push** ke variabel item, dan lalu variabel **item** akan bernilai dan nilai tersebut kita set di Local Storage localData kita.

**ELSE statement** jika ternyata sudah terdapat **Local Storage localData** maka hal yang pertama kita lakukan adalah variabel **item** di isi dengan data **Local Storage** kita, lalu hasil dari **inputValue** di **pus** ke dalam variabel **item** dan set **localData** dengan nilai semua **item** yang ada.

langkah selanjutnya adalah mengubah **state** kita, menapga kita perlu mengubah state kita padahal kita sudah menyimpannya di **Local Storage**? well karena kita perlu untuk menampilkan isi data kita dan terlebih dalam **React** haram hukumnya untuk mengganti state tanpa _syntax _**setState\(\).**

```jsx
//...
this.setState({
    items: JSON.parse(localStorage.getItem('localData')),
});

this.input.value = '';
this.input.focus();
//...
```

Dapat di lihat pada kodingan di atas bahwasanya kita me-**reset** inputan kita setelah kita menjalankan **onAddHandler** dan memberikan fungsi **fokus** pada inputan kita. Dengan hal tersebut maka selesailah fungsi untuk menambahkan list baru pada aplikasi TodoList kita, akan tetapi fungsi tersebut harus di deklarasikan terhadap button dan input agar dapat di jalankan.

```jsx
//...
<input
    className="form-control"
    type="text"
    placeholder="insert todo list"
    ref={el => (this.input = el)}
/>
<button
    className="btn btn-outline-primary"
    onClick={this.onAddHandler}>
    Add Todo List
</button>
//...
```

Pada react kita mempunyai beberapa cara untuk meng-handle event, untuk info lengkap bisa cek [di sini](https://reactjs.org/docs/handling-events.html). Mari kita bahas button terlebih dahulu, kita mempunyai sebuah button dengan event `onClick`, jadi ketika kita mengklik button tersebut maka fungsi **onAddHandler** akan di jalankan dan lalu untuk meng-ikat inputan kita membutuhkan `ref` singkatan dari references dan info lengkap klik [di sini](https://reactjs.org/docs/refs-and-the-dom.html). Secara singkatnya, apa yang di dapat pada input akan di ubah menjadi sebuah fungsi yang mengubah value inputan menjadi variabel **el**, dan **el** tersebut akan di pass ke `this.input` itulah mengapa di fungsi **onAddHandler** kita mempunyai `let inputValue = this.input.value` 

Sekarang coba kalian ketik dalam tag input dan klik tombol add Button dan apa yang terjadi? nothing happens right? well, itu karena kita belum merender hasil dari state **items** kita ke UI kita, akan tetapi jika kalian melakukan inspect element \(Ctrl + Shift + I\) dan kalian arahkan pada tab storage kalian akan melihat hasl dari yang kalian inputkan. Dengan begitu selesai sudah fungsi **onAddHandler** kita

#### onRemoveHandler

Fungsi ini cukup **to the point** cukup merujuk pada list mana yang akan di delete dan ketika tombol di delete maka list tersebut akan hilang.

```jsx
//...
onRemoveHandler = id => {
    let updatedTodo = this.state.items.filter((todo, index) => {
      return (
        id !== index
      );
    });
    localStorage.setItem('localData', JSON.stringify(updatedTodo));
    this.setState({
      items: updatedTodo,
    });
  };
  //..
```

fungsi kita akan menerima **id** sebagai pointer list lalu di dalam fungsi kita, kita deklarasikan variabel **updatedTodo** yang mempunyai fungsi filter referensi klik [di sini](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter). Sederhananya fungsi filter akan mencek **id** yang di dapat dengan **index** apakah tidak sama jika tidak sama maka kembalikan nilai **TRUE,** maka yang di klik akan menghiland dari list kita dan sisanya akan tetap ada.

Langkah selanjutnya adalah kita mengubah **Local Storage localData** kita dengan hasil terbaru dari **updatedTodo** dan mengubah state **items** kita sesuai todo yang tersisa.

```jsx
<ListGroup>
  {this.state.items &&
    this.state.items.map((item, index) => {
    return (
      <ListGroupItem key={index}>
        <div className="d-flex">
          <div className="p-2">{item}</div>
          <div className="ml-auto p-2">
            <button
              className="btn btn-danger"
              onClick={() => this.onRemove(index)}>
              X
            </button>
          </div>
        </div>
      </ListGroupItem>
    );
  })}
</ListGroup>
```

Pertama kita mengecek apakah `this.state.items` mempunyai data, jika mempunyai data maka `this.state.items.map` akan dijalankan fungis map ini akan mengitaris array dari state items kita, fungsi `.map` mempunyai 2 argumen, **item** dan **index** item berguna untuk meng-outputkan setiap data sedangkan index berguna untuk menjadi pointer pengarah dari list kita yang nantinya akan berguna untuk membantu proses pen-deletean list kita. lalu kita mempunyai tombol dengan event `onClick` berhubung penggunaan `this.onRemove` tidak akan bergunsi dikarenakan terdapat argumen yang harus di parsing, maka dari itu kita menggunakan **ES6 Arrow function,** untuk membantu menyelesaikan masalah ini, dan voila buat list baru dan klik tombol X dan apa yang terjadi? yap list kita menghilang. 

Dengan demikian selesai sudah aplikasi sederhana TodoList kita, dari program simple ini kita sudah bisa mendapat fondasi dasar untuk membuat aplikasi sederhana React karena sudah mencangkup CRD sederhana \(Create, Read, dan Delete\).  

