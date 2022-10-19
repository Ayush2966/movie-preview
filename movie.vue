<template>
  <div id="app">
    <div class="container">
      <header class="header">
        <h1 class="logo">
          Movies
          <div class="period"></div>
        </h1>

        <div class="avatar"></div>
      </header>

      <div class="stack">
        <VueCardStack
          :cards="cards"
          :cardWidth="286"
          :cardHeight="458"
          stackWidth="100%"
          @move="onMove"
          @update="onUpdate"
        >
          <template v-slot:card="{ card }">
            <div class="card">
              <img
                onmousedown="event.preventDefault()"
                draggable="false"
                :src="card.cover"
                :style="{
                  ...getStyles(card),
                  transition: card.isDragging ? 'none' : 'all 0.2s',
                }"
              />
            </div>
          </template>
        </VueCardStack>
      </div>

      <footer class="footer">
        <div
          class="info"
          v-for="(card, index) in cards"
          :key="index"
          :style="getInfoStyles[index]"
        >
          <h2 class="title">{{ card.title }}</h2>
          <h3 class="date">
            Coming in
            <span>{{ card.release }}</span>
          </h3>
        </div>
        <div class="rating">
          <div
            v-for="x in 5"
            :key="x"
            class="star"
            v-bind:class="{ active: x <= activeCard.rating }"
          />
        </div>
      </footer>
    </div>

    <div class="try-me">
      <svg viewBox="0 0 24 24" width="20" height="20">
        <path
          d="M13.5091 24.0063C10.7699 23.971 8.15366 22.8634 6.22167 20.9213C6.03942 20.7406 5.88216 20.5364 5.75406 20.314L1.88564 13.6338C1.84853 13.569 1.81995 13.4996 1.80062 13.4274C1.64142 12.893 1.66621 12.3206 1.87102 11.802C2.07583 11.2833 2.4488 10.8485 2.93017 10.567C3.29921 10.353 3.71811 10.2399 4.14475 10.2391V10.2391L0.179159 3.3646C0.141029 3.30022 0.112394 3.23068 0.0941387 3.15812C-0.05595 2.62723 -0.0259149 2.06157 0.179549 1.54956C0.385013 1.03755 0.754327 0.608036 1.22977 0.328161C1.71192 0.0542366 2.27111 -0.0525986 2.82028 0.0242861C3.36946 0.101171 3.87781 0.357462 4.2662 0.753262C4.31781 0.806246 4.36269 0.865397 4.39981 0.929376L7.76418 6.75934C7.97368 6.39282 8.27508 6.08723 8.63867 5.8727C9.07352 5.62113 9.57567 5.51057 10.076 5.55624C10.5763 5.60192 11.0501 5.80159 11.4322 6.12776C11.6456 5.69776 11.9816 5.34063 12.3978 5.10144C12.8357 4.85175 13.3405 4.74449 13.8421 4.79455C14.3438 4.8446 14.8174 5.04948 15.1974 5.38079C15.4098 4.94855 15.7459 4.58921 16.163 4.3484C16.6459 4.07256 17.2073 3.96635 17.7576 4.0467C18.3079 4.12704 18.8155 4.38933 19.1994 4.79172C19.2523 4.84132 19.2973 4.89865 19.333 4.96177L22.5881 10.6035C22.718 10.8241 22.8181 11.061 22.8856 11.3079C24.1002 16.1662 22.1265 20.5448 18.3674 22.7189C16.8889 23.5667 15.2134 24.0107 13.5091 24.0063ZM4.14475 11.6601C3.93233 11.6614 3.72358 11.7157 3.53746 11.818C3.39879 11.8979 3.27727 12.0044 3.17988 12.1313C3.08248 12.2583 3.01114 12.4033 2.96994 12.5579C2.92875 12.7125 2.91851 12.8737 2.93983 13.0323C2.96114 13.1909 3.01358 13.3437 3.09414 13.482L6.13058 18.7411C7.25801 20.694 9.11506 22.119 11.2932 22.7027C12.3717 22.9917 13.4965 23.0655 14.6035 22.9198C15.7105 22.774 16.778 22.4117 17.7449 21.8535C18.7119 21.2952 19.5595 20.552 20.2392 19.6662C20.9189 18.7804 21.4175 17.7694 21.7065 16.6909C21.9955 15.6124 22.0693 14.4875 21.9236 13.3805C21.7779 12.2735 21.4155 11.2061 20.8573 10.2391L18.4281 6.03059C18.2601 5.76622 17.9964 5.57701 17.6921 5.50248C17.3878 5.42795 17.0665 5.47387 16.7953 5.63064C16.5241 5.78741 16.324 6.04291 16.2367 6.34377C16.1494 6.64464 16.1817 6.96759 16.3269 7.24517C16.4075 7.38529 16.429 7.55167 16.3869 7.70769C16.3448 7.86372 16.2424 7.99662 16.1022 8.07715C15.9621 8.15768 15.7957 8.17925 15.6397 8.13711C15.4837 8.09497 15.3508 7.99258 15.2703 7.85245L14.663 6.80185C14.5857 6.66179 14.4806 6.53895 14.3543 6.4408C14.228 6.34266 14.083 6.27127 13.9281 6.231C13.6162 6.15185 13.2859 6.19532 13.0051 6.35245C12.8668 6.4327 12.7457 6.53946 12.6488 6.6666C12.5519 6.79374 12.481 6.93876 12.4403 7.09334C12.3988 7.24763 12.3882 7.40859 12.4091 7.56698C12.4299 7.72538 12.4818 7.87811 12.5617 8.01642C12.6344 8.15521 12.6505 8.31669 12.6069 8.46712C12.5632 8.61755 12.4632 8.7453 12.3276 8.82368C12.1919 8.90207 12.0313 8.92503 11.8792 8.88776C11.727 8.8505 11.5952 8.75589 11.5111 8.62371V8.62371L10.9039 7.56703C10.8236 7.42876 10.7169 7.3077 10.5897 7.21078C10.4626 7.11386 10.3175 7.043 10.163 7.00225C9.85324 6.9202 9.52364 6.96385 9.24596 7.12371C9.10657 7.20335 8.98434 7.30983 8.88636 7.437C8.78837 7.56417 8.71657 7.70951 8.67511 7.8646C8.59306 8.17432 8.63671 8.50392 8.79657 8.7816V8.7816C8.86917 8.92039 8.88534 9.08187 8.8417 9.2323C8.79806 9.38273 8.69798 9.51048 8.56237 9.58887C8.42676 9.66725 8.26611 9.69021 8.11398 9.65295C7.96184 9.61568 7.82999 9.52108 7.74596 9.38889L3.49495 2.02857C3.33368 1.75015 3.06853 1.54709 2.7577 1.46398C2.44687 1.38086 2.11576 1.42447 1.83705 1.58525C1.69691 1.66755 1.57472 1.77715 1.47771 1.90754C1.38069 2.03794 1.31084 2.18647 1.27228 2.34436C1.23082 2.49864 1.2202 2.6596 1.24105 2.81799C1.26189 2.97639 1.31377 3.12912 1.39373 3.26743L6.25203 11.648V11.6844L8.37754 15.3646C8.41769 15.4339 8.44374 15.5105 8.45417 15.5899C8.46461 15.6693 8.45923 15.75 8.43834 15.8274C8.41746 15.9047 8.38148 15.9771 8.33248 16.0405C8.28348 16.1039 8.22243 16.1569 8.15284 16.1966C8.08131 16.2387 8.0019 16.2658 7.91951 16.276C7.83712 16.2863 7.7535 16.2795 7.67382 16.2562C7.59414 16.2329 7.52009 16.1934 7.45625 16.1404C7.39241 16.0873 7.34013 16.0217 7.30264 15.9476L5.18321 12.2431C5.02183 11.9746 4.76291 11.7787 4.46054 11.6966C4.35706 11.6721 4.25107 11.6599 4.14475 11.6601V11.6601Z"
          fill="currentColor"
        />
      </svg>
      <p>Try Me</p>
    </div>

    <a
      class="attribution"
      href="https://dribbble.com/shots/3982621-InVision-Studio-Movies-app-concept"
      target="_blank"
    >
      <img src="https://assets.codepen.io/152347/dribbble.svg?abc" alt="" />
      <div class="inner">
        <h3>Charles Patterson</h3>
        <h5>Movies app concept</h5>
      </div>
    </a>
  </div>
</template>
  
  <script>
import VueCardStack from "https://assets.codepen.io/152347/vue-card-stack.esm.js?skahdfjahj";

export default {
  components: {
    VueCardStack,
  },
  computed: {
    getInfoStyles() {
      const SPEED = 0.25;

      if (this.stack.length) {
        const styles = this.cards.map((card, index) => {
          const cardInStack = this.stack.find((x) => x.id === card.id);
          const cardIndexInStack = this.stack.findIndex(
            (x) => x.id === card.id
          );

          const yPosFactor = this.convertRange(
            cardInStack.xPos,
            [0, 69],
            [1, 0]
          );

          if (this.direction === "left") {
            const o = (i) => {
              if (this.isDragging) {
                if (i === 0) {
                  return this.convertRange(cardInStack.xPos, [380, 69], [0, 1]);
                } else if (i === 1) {
                  return this.convertRange(cardInStack.xPos, [69, 65], [1, 0]);
                } else {
                  return 0;
                }
              } else {
                return i === 1 ? 1 : 0;
              }
            };

            return {
              transform: `translate(0, ${
                cardIndexInStack === 0
                  ? 5 * yPosFactor
                  : (cardIndexInStack + 1) * (100 * yPosFactor)
              }px)`,
              transition: `transform ${
                this.isDragging && !(cardIndexInStack > 1) ? 0 : SPEED
              }s ease 0s, opacity ${this.isDragging ? 0 : SPEED}s ease 0s`,
              opacity: o(cardIndexInStack),
            };
          } else {
            const o = (i) => {
              if (this.isDragging) {
                if (i === 1) {
                  return this.convertRange(cardInStack.xPos, [69, 200], [1, 0]);
                } else if (i === 2) {
                  return this.convertRange(cardInStack.xPos, [55, 69], [0, 1]);
                } else {
                  return 0;
                }
              } else {
                return i === 1 ? 1 : 0;
              }
            };

            return {
              transform: `translate(0, ${
                cardIndexInStack === 1
                  ? 5 * yPosFactor
                  : (cardIndexInStack - 1) * (100 * yPosFactor)
              }px)`,
              transition: `transform ${
                this.isDragging && !(cardIndexInStack > 2) ? 0 : SPEED
              }s ease 0s, opacity ${this.isDragging ? 0 : SPEED}s ease 0s`,
              opacity: o(cardIndexInStack),
            };
          }
        });

        return styles;
      }

      return {};
    },
  },
  methods: {
    convertRange(value, r1, r2) {
      return ((value - r1[0]) * (r2[1] - r2[0])) / (r1[1] - r1[0]) + r2[0];
    },
    getStyles(card) {
      const MIN_POS_X = 0;
      const MAX_POS_X = 69;
      const MIN_SCALE_FACTOR = 1;
      const MAX_SCALE_FACTOR = 2.5;

      const opacity = this.convertRange(
        card.xPos > 0 ? card.xPos : 0,
        [MIN_POS_X, MAX_POS_X],
        [0, 1]
      );

      const scale = this.convertRange(
        card.xPos > 0 ? card.xPos : 0,
        [MIN_POS_X, MAX_POS_X],
        [MAX_SCALE_FACTOR, 1]
      );

      const normalizedScale =
        scale >= MIN_SCALE_FACTOR
          ? scale <= MAX_SCALE_FACTOR
            ? scale
            : MAX_SCALE_FACTOR
          : MIN_SCALE_FACTOR;

      const normalizedOpacity = opacity >= 0 ? (opacity <= 1 ? opacity : 1) : 0;

      return {
        transform: `scale(${card.$index !== 0 ? normalizedScale : 1}, ${
          card.$index !== 0 ? normalizedScale : MIN_SCALE_FACTOR
        })`,
        opacity: `${card.$index !== 0 ? normalizedOpacity : 1}`,
      };
    },
    onUpdate({ active }) {
      this.activeCard = active;
    },
    onMove({ stack, direction, isDragging }) {
      this.stack = stack;
      this.direction = direction;
      this.isDragging = isDragging;
    },
  },
  data() {
    return {
      stack: [],
      isDragging: false,
      activeCard: {
        rating: 0,
      },
      cards: [
        {
          id: 1,
          title: "The Martian",
          release: "10.2.18",
          cover: "https://assets.codepen.io/152347/the-martian.jpg",
          rating: 3,
          description:
            "A young blade runner's discovery of a long-buried secret leads him to track down former blade runner Rick Deckard, who's been missing for thirty years.",
        },
        {
          id: 2,
          title: "Blade Runner 2049",
          release: "10.6.17",
          cover: "https://assets.codepen.io/152347/blade-runner.jpg",
          rating: 5,
          description:
            "A young blade runner's discovery of a long-buried secret leads him to track down former blade runner Rick Deckard, who's been missing for thirty years.",
        },
        {
          id: 3,
          title: "Justice League",
          release: "10.6.17",
          cover: "https://assets.codepen.io/152347/justice-league.jpg",
          rating: 1,
          description:
            "A young blade runner's discovery of a long-buried secret leads him to track down former blade runner Rick Deckard, who's been missing for thirty years.",
        },
        {
          id: 4,
          title: "Ex Machina",
          release: "10.6.17",
          cover: "https://assets.codepen.io/152347/ex-machina.jpg",
          rating: 4,
          description:
            "A young blade runner's discovery of a long-buried secret leads him to track down former blade runner Rick Deckard, who's been missing for thirty years.",
        },
        {
          id: 5,
          title: "Alien",
          release: "10.6.17",
          cover: "https://assets.codepen.io/152347/alien.jpg",
          rating: 3,
          description:
            "A young blade runner's discovery of a long-buried secret leads him to track down former blade runner Rick Deckard, who's been missing for thirty years.",
        },
        {
          id: 6,
          title: "Interstellar",
          release: "10.6.17",
          cover: "https://assets.codepen.io/152347/interstellar.jpg",
          rating: 5,
          description:
            "A young blade runner's discovery of a long-buried secret leads him to track down former blade runner Rick Deckard, who's been missing for thirty years.",
        },
      ],
    };
  },
};
</script>
  
  <style lang="scss">
@font-face {
  font-family: "Eina";
  src: url("https://assets.codepen.io/152347/eina-03-semibold.woff2")
      format("woff2"),
    url("https://assets.codepen.io/152347/eina-03-semibold.woff") format("woff");
}

body {
  font-family: "Eina", serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: #9796f0; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #fbc7d4,
    #9796f0
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #fbc7d4,
    #9796f0
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

#app {
  color: #1c1c1c;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  grid-gap: 60px;
}

.attribution {
  text-decoration: none;
  width: auto;
  border-radius: 999em;
  border: 1px solid rgba(255, 255, 255, 0.15);
  padding: 12px 25px 12px 60px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #1c1c1c;
  background: #fff;
  background: rgba(255, 255, 255, 0.25);
  position: relative;
  font-family: "Montserrat", sans-serif;

  > img {
    color: green;
    width: 40px;
    height: 40px;
    position: absolute;
    left: 8px;
    top: 50%;
    transform: translate(0, -50%);
  }

  .inner {
    display: flex;
    flex-direction: column;
    grid-gap: 2px;
  }

  h3 {
    font-size: 11px;
  }

  h5 {
    font-size: 16px;
    font-weight: 600;
  }
}

.container {
  z-index: 2;
  position: relative;
  background: #fff;
  border-radius: 12px;
  width: 375px;
  height: 667px;
  overflow: hidden;
  box-shadow: 0 70px 63px -60px;
  display: flex;
  flex-direction: column;
}

//////////////////////////////////////////
// HEADER
//////////////////////////////////////////
.header {
  display: flex;
  justify-content: space-between;
  padding: 24px;

  .logo {
    align-items: baseline;
    display: flex;
    font-size: 29px;
  }

  .period {
    width: 5px;
    height: 5px;
    margin-left: 4px;
    background: #ff3366;
  }

  .avatar {
    width: 32px;
    height: 32px;
    border-radius: 20px;
    background: #ffe259;
    overflow: hidden;
  }

  .avatar > img {
    width: 100%;
    height: auto;
  }
}

//////////////////////////////////////////
// FOOTER
//////////////////////////////////////////
.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 24px;
  flex-direction: row-reverse;
  position: relative;
}

//////////////////////////////////////////
// RATING
//////////////////////////////////////////
.rating {
  display: flex;
  justify-content: space-between;
  justify-self: flex-end;

  .star {
    background: url("https://assets.codepen.io/152347/star.svg");
    background-size: 100%;
    width: 15px;
    height: 15px;
    transform: scale(0.5, 0.5);
    margin: 2px;
    transition: all 0.2s ease;
    opacity: 0.5;

    &.active {
      transform: scale(1, 1);
      opacity: 1;
    }
  }
}

//////////////////////////////////////////
// INFO
//////////////////////////////////////////
.info {
  position: absolute;
  left: 24px;

  .title {
    font-size: 16px;
    line-height: 22px;
  }

  .date {
    font-size: 11px;
    line-height: 15px;
    color: #9ba7c6;
  }
}

.stack {
  width: 100%;
}

.card {
  width: 100%;
  height: 100%;
  border-radius: 8px;
  overflow: hidden;
  transform: rotate(0);
  background: #fff;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -o-user-select: none;
  user-select: none;

  > img {
    width: 286px;
    height: 458px;
    object-fit: cover;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -o-user-select: none;
    user-select: none;
  }
}

.try-me {
  display: flex;
  grid-gap: 8px;
  align-items: center;
  font-size: 14px;
  margin-top: -24px;
  font-family: "Montserrat", sans-serif;
  font-weight: 600;
  color: #fff;
}
</style>
  