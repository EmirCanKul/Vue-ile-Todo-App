<template>
  <main id="todolist">
    <h1>Yapılacaklar Listesi <span>REACT İÇİN JAVASCRİPT</span></h1>
    <!-- Eğer karakter sayım 0'dan büyükse göster, p'nin içinde yazdığım gibi v-else yani değilse gösterme -->
    <div class="items" v-if="items.length"> 
        <ul>
          <!-- Benim item'larımın döneceği yer li' dir. Aşağıda v-for ile datada yazdığım items'larımın içinden item'ları döndür dedim. Bunu döndürdüğüm için bir key değerine eşit olmalı. Aynı zamanda classı dinamik olarak
          çektim ve done classını ekle eğer item içinde ki done true'ya eşitse.-->
            <li v-for="(item, index) in items" :key="index" :class="{'done' : item.done}">
                <span class="label">{{item.title}}</span>
                <div class="actions">
                  <!-- Alttaki click' in amacı yapıldı olarak işaretliyse tersine, yapılmadı olarak işaretlediyse yine tersine çevirme olacak.  -->
                    <button class="btn-picto" type="button" @click="changeStatus(index)">
                      <!-- Yapıldığında dönecek ikonum alttaki, yanında da şunu demek istedik. Eğer item içinde ki done true' ya eşitse checkbox' ı göster, değilse 'check_box_outline_blank' bunu göster.  -->
                      <i aria-hidden="true" class="material-icons">{{item.done ? 'check_box' : 'check_box_outline_blank'}}</i> 
                      <!-- Yapılmadığında dönecek ikonum alttaki -->
                      <i aria-hidden="true" class="material-icons"></i>
                    </button>
                    <button class="btn-picto" type="button"
                            aria-label="Delete" title="Delete" @click="deleteItem(index)">
                            <!-- Yukarıda silme işlemi için delete butonumu oluşturup delete methodumu çalıştırdım. Hangi öğenin silineceğini bilmek için index atadım. -->
                        <i aria-hidden="true" class="material-icons">delete</i>
                    </button>
                </div>
            </li>
        </ul>
    </div>

    <!-- <p class="emptylist" v-else>Yapılacaklar listeniz boş.</p> -->


    <!-- Burada click yerine submit kullanıp sadece click ettiğimize değilde enter'a bastığımızda da öğenin eklenmesini isediğimiz için submit kullanıyoruz.Bizi yönlendirmesini engellemek 
    içinde .prevent kullanıyoruz.-->
    <form @submit.prevent="addNewItem">
        <label for="newitem">Yapılacaklar Listesine Ekle.</label>
        <!-- Benim verim güncellendiğinde otomatik olarak newItemTitle' ında güncellenmesi lazım. Bu yüzden v-model kullanıyoruz.  -->
        <input type="text" name="newitem" id="newitem" v-model="newItemTitle"> 
        <button type="submit">Add item</button>
    </form>
</main>
</template>



<script>

    export default {
      data(){
        return {
          newItemTitle: '', //Yazıp eklediğimiz verilerimizi tuttuğumuz yer.
          items : [], //Yapılacaklarımızı sakladığımız yer
        }
      },
      methods:{
        addNewItem(){ //Öğe eklemenpm
          if (this.newItemTitle === ''){ //Eğer newItemTitle değeri boşsa demek istedik.
          return //Sadece döndür, bu sayede boş item ekleyemiyoruz.
        }
          this.items.push({
            title : this.newItemTitle,
            done: false,  //Yapılıp, yapılmadığının durumunu tutan bir şart eklemem gerekiyor.
          });
          this.newItemTitle= ''; //BUrada önce bir üstte item ' ı ekler sonrasında içini boşaltmak için yazdık.
        },
        deleteItem(index){ 
          this.items.splice(index,1); //Burada benim items dizinimden denk gelen index değerimden bir tanesini sil.
        },
        changeStatus(index){
          const item =this.items[index]; //İtemi değişkene taşıdık.
          //this.items[index].done = true; //İtems değerlerimden indexe eşit olan veriye gel bunun değerini true yap.
          item.done = !item.done; //Eğer true' ya eşitse bunu false'a çevir eğer tersi ise tersine çevir.
        },
      
       
      },
      mounted(){
          const initItems = localStorage.getItem('todo');
          const items =JSON.parse(initItems ? initItems : '[]'); //Önce texten tekrar jsona çevirdik. Sonra bana eğer veri varsa todo getir yoksa boş getir dedik.
           this.items = items;                                                          //Ve burada ki amacım sayfa render olduğunda verilerim kaybolmasın.
        },
      watch : {
          items : {
            deep :true,
            handler(){
              const items = this.items;
              localStorage.setItem('todo', JSON.stringify(items)); //Burada json.stringify ile items ' ıtext haline getiriyorum. 
            }
          }
        }
    }
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  background: #f7f1f1;
  font-size: 1.1rem;
  font-family: 'Quicksand', sans-serif;
  height: 100%;
}

@keyframes strikeitem {
  to {
      width: calc(100% + 1rem);
  }
}

#todolist {
  margin: 4rem auto;
  padding: 2rem 3rem 3rem;
  max-width: 500px;
  background: #FF5E5E;
  color: #FFF;
  box-shadow: -20px -20px 0px 0px rgba(100, 100, 100, .1);
}

#todolist h1 {
  /*text-align:center;*/
  font-weight: normal;
  font-size: 2.6rem;
  letter-spacing: 0.05em;
  border-bottom: 1px solid rgba(255, 255, 255, .3);
}

#todolist h1 span {
  display: block;
  font-size: 0.8rem;
  margin-bottom: 0.7rem;
  margin-left: 3px;
  margin-top: 0.2rem;
}

#todolist .emptylist {
  margin-top: 2.6rem;
  text-align: center;
  letter-spacing: .05em;
  font-style: italic;
  opacity: 0.8;

}

#todolist ul {
  margin-top: 2.6rem;
  list-style: none;
}

#todolist .todolist-move {
  transition: transform 1s;
}

#todolist li {
  display: flex;
  margin: 0 -3rem 4px;
  padding: 1.1rem 3rem;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
}

#todolist .actions {
  flex-shrink: 0;
  padding-left: 0.7em;
}

#todolist .label {
  position: relative;
  transition: opacity .2s linear;
}

#todolist .done .label {
  opacity: .6;
}

#todolist .done .label:before {
  content: '';
  position: absolute;
  top: 50%;
  left: -.5rem;
  display: block;
  width: 0%;
  height: 1px;
  background: #FFF;
  animation: strikeitem .3s ease-out 0s forwards;
}

#todolist .btn-picto {
  border: none;
  background: none;
  -webkit-appearance: none;
  cursor: pointer;
  color: #FFF;
}


/* FORM */
form {
  margin-top: 3rem;
  display: flex;
  flex-wrap: wrap;
}

form label {
  min-width: 100%;
  margin-bottom: .5rem;
  font-size: 1.3rem;
}

form input {
  flex-grow: 1;
  border: none;
  background: #f7f1f1;
  padding: 0 1.5em;
  font-size: initial;
}

form button {
  padding: 0 1.3rem;
  border: none;
  background: #FF6666;
  color: white;
  text-transform: uppercase;
  font-weight: bold;
  border: 1px solid rgba(255, 255, 255, .3);
  margin-left: 5px;
  cursor: pointer;
  transition: background .2s ease-out;
}

form button:hover {
  background: #FF5E5E;
}

form input,
form button {
  font-family: 'Quicksand', sans-serif;
  height: 3rem;
}
</style>
