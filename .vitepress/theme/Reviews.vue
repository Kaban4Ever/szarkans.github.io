<template>
  <div class="reviews-container">
    <div class="review-wrapper">
      <transition name="fade">
        <div class="review" v-if="currentReview">
          <div class="review-header">
            <img class="review-avatar" :src="currentReview.avatar" alt="Автор" />
            <span class="review-name">{{ currentReview.name }}</span>
          </div>
          <Divider type="solid" />
          <div class="review-text">{{ currentReview.text }}</div>
        </div>
      </transition>

      <!-- Кнопка "Назад" -->
      <button class="review-button review-button-left" @click="prevReview"><i class="pi pi-chevron-left"></i></button>
      <!-- Кнопка "Вперёд" -->
      <button class="review-button review-button-right" @click="nextReview"><i class="pi pi-chevron-right"></i></button>
    </div>
  </div>
</template>

<script setup>
// Допустим, у нас есть массив отзывов.
// В реальности вы можете импортировать его или получить из API.
const reviews = [
  {
    avatar: 'https://cravatar.eu/avatar/Szarkan/50.png', // Замените на вашу ссылку
    name: 'Не Szarkan',
    text: 'сервер оч крутой. я не админ.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/sm1lly/50.png', // Замените на вашу ссылку
    name: 'sm1lly',
    text: 'норм. я не админ.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Ystika/50.png', // Замените на вашу ссылку
    name: 'Ystika',
    text: 'Вчера я играл на кошкокрафте до 2 часов ночи, было очень весело... Хохотал на весь дом всю ночь, но сегодня почему-то проснулся в детдоме.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Sibirius/50.png', // Замените на вашу ссылку
    name: 'Sibirius',
    text: 'КошкоКрафт очень крутой сервер. Здесь добрие и веселые котята, хорошие ивенты, имеется много всяких кастомных приколюх, особенно сильно мне нравица мебель и рисовать. А так же, только здесь самая лучшая, отзывчивая и пиздец какая крутая модерация, особенно Всекотец(ака админ). Грустно, что имеются приваты, т.к нельзя воровать ресы и гриферить чужие постройки. Я рыгнул и у меня упал потолок.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/dark_mooon/50.png', // Замените на вашу ссылку
    name: 'dark_moooon',
    text: 'Кошкокрафт это мой второй сервер, где я остался окончательно. Честно, не могу сказать, почему я выбрал именно его. Но могу быть уверенным, что здесь много интересных людей, с которыми можно потерять свою жизнь, играя в майнкрафт. Я играю на сервере год. У меня было много этапов развития. Сначала я пытался сделать свой клан Catstadt, потом пошёл в министры, и начал строить спавн. Я был в ренегатов, в кайфограде. И это всё привело меня к тому, чем я сейчас являюсь. Я уверен, что перетерплю ещё больше таких этапов, но одно останется неизменным - это самый крутой и кошачий сервер, под названием "Catcraft"'
  },
  {
    avatar: 'https://cravatar.eu/avatar/CharaBell/50.png', // Замените на вашу ссылку
    name: 'CharaBell',
    text: 'самый лучший сервер, у админа большой хуй. я не жена админа.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Kaban4Ever/50.png', // Замените на вашу ссылку
    name: 'Kaban4Ever',
    text: 'Играю с 5 сезона, название сервер оправдывает. Коты везде и повсюду, даже в интерфейсе... За время игры познал себя  в Блендере, т.к. нашел некое вдохновение. Тут играют ОЧЕНЬ интересные люди, непредсказуемые. В целом тут все вежливые и дружелюбные, я даже нашел себе друга, с которым общаюсь и играю по сей день. Сервер регулярно обновляют и завозят новый контент. А ещё можно набухаться.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Jodich/50.png', // Замените на вашу ссылку
    name: 'Jodich',
    text: 'Кошкокрафт- лучший и единственный кошачий сервер, который когда либо встречал. Там мне очень нравится строить ивенты, осваивать кастомные механики, и ваще взаимодействовать с людьми. Особенно мне нравится варить одуванчиковое вино из бревери, ставлю серверу 993 из 10 балов. 👍. Однако хромает сюжет'
  },
  {
    avatar: 'https://cravatar.eu/avatar/l_vitek_l/50.png', // Замените на вашу ссылку
    name: 'l_vitek_l',
    text: 'СНРВЕР ДАСТАТАЧНА ХАРОШЫЙ ДЛЯ ИГРЫ С ДРУЗЬЯМИ, НО ЭТО НЕ ТОЧНО Т.К ИЗ ДРУЗЕЙ У МЕНЯ ТОЛЬКА ПИСЬКА С КОТОРОЙ Я ИНАГДА ЕГРАЮ. Ладно потом что-то нормальное придумаю'
  },
  {
    avatar: 'https://cravatar.eu/avatar/TM_Kocherga/50.png', // Замените на вашу ссылку
    name: 'TM_Kocherga',
    text: 'Кошкокрафт хороший сервер,играю с 5 и атмосферка хорошая,сервер оч интересный!!Всем советую'
  },
  {
    avatar: 'https://cravatar.eu/avatar/artlaks/50.png', // Замените на вашу ссылку
    name: 'artlaks',
    text: 'До того как я узнал о Кошкокрафте, у меня была импотенция. Но когда я узнал о нем и начал играть, мой хуй стоял как каменный! После двух месяцев игры он выпал и улетел в поисках женщины. Спустя еще месяц он вернулся и заявил, что та, кого он нашел, беременна от него! Так я и стал дедом в 16 лет. А еще отец вернулся с 2 тоннами хлеба и 100 000 литрами молока, чтобы больше никогда не пропадать на столь долгий срок. В общем, сервер объединяет ранее незнакомых людей в семьи, а некоторые из семей возобновляет вновь. 💯 /10'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Charanixy/50.png', // Замените на вашу ссылку
    name: 'Charanixy',
    text: 'всем привет'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Kvar1kc/50.png', // Замените на вашу ссылку
    name: 'Kvar1kc',
    text: 'Играю с 5-го сезона. Блин Очень круто!  Сервер всегда находится в онлайн! Игроки отзывчивые Админ если ничего не делает новый фишки и круты дополнение на Кошкокрафте ( В крайних случаях) то ГыЧу мафонит.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Mr_Milota/50.png', // Замените на вашу ссылку
    name: 'Mr_Milota',
    text: 'Под пивко сойдёт, а под дилдо заходит😼'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Mrs_Tori/50.png', // Замените на вашу ссылку
    name: 'Mrs_Tori',
    text: 'Сервер хорош каждую неделю выходят разоблачения и интриги перестала смотреть криминальную Россию так как и на Кошкокрафте хватает запутанных историй. Если спросят чего я хочу в свои 20 так это три заркамбалы с пивом и шалкер гычи под кроватью.'
  },
  {
    avatar: 'https://cravatar.eu/avatar/Tommy_Wer/50.png', // Замените на вашу ссылку
    name: 'Tommy_Wer',
    text: 'Тот самый сервер, где за один день ты сможешь покатать 20 человек у себя на голове ради прикола, устроить шумную свадьбу со взрывом в конце, и это только за первые 3 часа. Всем доволен и не жалею о своём приходе на проект 👍'
  },
];

// Текущий индекс отзыва
import { ref, computed } from 'vue';

const currentIndex = ref(0);

function nextReview() {
  currentIndex.value = (currentIndex.value + 1) % reviews.length;
}

function prevReview() {
  currentIndex.value = (currentIndex.value - 1 + reviews.length) % reviews.length;
}

const currentReview = computed(() => reviews[currentIndex.value]);


// Добавим простой функционал свайпа для мобильных устройств
// (Это базовый пример. В реальности стоит более гибко обрабатывать расстояния свайпа.)
let startX = 0;

function handleTouchStart(e) {
  startX = e.touches[0].clientX;
}

function handleTouchEnd(e) {
  const endX = e.changedTouches[0].clientX;
  const diff = endX - startX;
  // Если смахнули влево больше чем на 50px — следующий отзыв
  if (diff < -50) {
    nextReview();
  }
  // Если смахнули вправо больше чем на 50px — предыдущий отзыв
  if (diff > 50) {
    prevReview();
  }
}
</script>

<style scoped>
.reviews-container {
  /* Центрируем по горизонтали */
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

.review-wrapper {
  position: relative;
  /* На десктопе 50% ширины родителя */
  width: 70%;
  /* background: #f9f9f9; */
  border: 3px solid #2828286d;
  border-radius: 15px;
  padding: 20px;
  box-sizing: border-box;
}

/* Адаптив для мобильных: при ширине экрана меньше 600px — 95% */
@media (max-width: 600px) {
  .review-wrapper {
    width: 95%;
  }
}

/* Стили для самого отзыва */
.review-header {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}
.review-avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}
.review-name {
  font-weight: bold;
  margin-left: 10px;
}

/* Кнопки навигации */
.review-button {
position: absolute;
  top: 0; /* Располагаем кнопку сверху контейнера */
  bottom: 0; /* Растягиваем до нижнего края */
  width: 40px; /* Задаём фиксированную ширину кнопки */
  border: none;
  font-size: 2rem;
  cursor: pointer;
  line-height: 1;
  display: flex; /* Центрируем содержимое кнопки */
  justify-content: center;
  align-items: center;
  opacity: 0.7;
  transition: opacity 0.3s, background 0.3s;
}
.review-button:hover {
  opacity: 1;
  background: rgba(53, 53, 53, 0.374); /* Увеличиваем видимость при наведении */
}
.review-button-left {
  left: 0;
}
.review-button-right {
  right: 0;
}

/* Анимация перехода между отзывами */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>

<!-- Добавляем обработчики тач-событий на обёртку -->
<script>
  // Дополнительно обернём в отдельный script, чтобы повесить события на корневой элемент.
  export default {
    mounted() {
      const wrapper = this.$el.querySelector('.review-wrapper');
      wrapper.addEventListener('touchstart', this.$options.setup.handleTouchStart);
      wrapper.addEventListener('touchend', this.$options.setup.handleTouchEnd);
    },
    unmounted() {
      const wrapper = this.$el.querySelector('.review-wrapper');
      wrapper.removeEventListener('touchstart', this.$options.setup.handleTouchStart);
      wrapper.removeEventListener('touchend', this.$options.setup.handleTouchEnd);
    }
  }
</script>
