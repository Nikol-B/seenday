<template >
  <main>
    <div class="blocks">
      <div class="left-block">

        <div class="block blockdowork">
          <h2 :class="{ 'text-bold': isBoldTitle }">Выгрузка</h2>
          <p>Выполняет работу:</p>
          <ul>
            <li>Собирает фотографии из заказов пользователей.</li>
            <li>Выгружает по папкам.</li>
          </ul>
        </div>
        <StatusCard />
        <div>
          <div v-for="item in items" :key="item.id"
            :class="{ 'color_success': status === 1, 'color_danger': status === 2 }">
            <div class="block" @click="clickHandler(item.id, item.event)">
              <p>Задача выполнена:<p class="text-bold">{{ item.date }}</p></p>
              <p>Статус задачи:<p class="text-bold">{{ item.status_text }}</p></p>
              <p>ID выгрузки:<p class="text-bold">{{ item.id }}</p></p>
              <p> {{ item.event.replace(/<span class="text-bold">/, "").replace("/", "").replace(/<span>/, "") }}</p>
              <p>Размер выгрузки:<p class="text-bold">{{ item.size }}</p></p>
            </div>
          </div>
        </div>
      </div>


      <div v-if="!show" class="notice">
        <p>Для того, чтобы посмотреть информацию о
        <p class="text-bold">выгрузке</p>, а также ее скачать, нажмите на требуюмую выгрузку в столбце слева</p>
      </div>
      <div v-if="show" class="dowlandLinkBlock">
        <div class="dowlandBlockTop">
          <div class="idItem">
            <p>{{ idItem }}</p>
          </div>
          <p class="itemEvent">{{ itemEvent.replace(/<span class="text-bold">/, "").replace("/", "").replace(/<span>/, "")
          }}</p>
          <button class="buttonX" @click="closeWindow()">X</button>
        </div>
        <div v-for="itemRight in newItems" :key="itemRight.id" style="background-color: white; padding: 15px;">
          <div>
            <p class="text-bold" style="font-size: 16px;">Ссылка для скачивания архива Выгрузки (.zip) </p><br>
            <a class="link"> https://seenday.com/{{ itemRight.download_link.replace(/\D/g, "") }}</a>
            <button ref="copyButton" class="span-link" @click="copyLink(itemRight.download_link)">cкопировать
              ссылку</button>
            <div v-if="showModal" @close="showModal = false">
              <h3>Ссылка скопирована</h3>
            </div>
          </div>
          <div class="divButtonClose">
            <button class="buttonClose" @click="closeWindow()">Закрыть</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>



<script >
import "./style.scss";

export default {
  data() {
    return {
      items: [],
      newItems: [],
      show: false,
      showModal: false,
    };
  },


  mounted() {
    fetch('https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get')
      .then((response) => response.json())
      .then((data) => {
        this.items = data.response.data;
        this.status = data.response.status;
        console.log(this.status);
      })
      .catch((error) => {
        console.log(error);
      });
  },


  methods: {
    clickHandler(itemId, event) {
      this.show = this.show === '!show' ? 'show' : '!show';
      this.idItem = itemId;
      this.itemEvent = event;
      console.log("You clicked the button!");
      fetch('https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get&unload_id=' + this.idItem)
        .then((response) => response.json())
        .then((data) => {
          this.newItems = data.response.data;
          })
        .catch((error) => {
          console.log(error);
        });
    },

    closeWindow() {
      this.show = this.show === 'show';
      console.log("You clicked!");
    },

    copyLink(e) {
      this.aaa = e.replace(/\D/g, "");
      this.lll = `https://seenday.com/` + this.aaa;
      navigator.clipboard.writeText(this.lll)
        .then(() => {
          this.showModal = true;
          setTimeout(() => {
            this.showModal = false;
          }, 2000);
        })
        .catch((err) => {
          console.error('Failed to copy: ', err);
        });
    }

  },

};

</script>



