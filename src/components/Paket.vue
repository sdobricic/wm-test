<template>
  <div>
    <div class="inner" v-bind:class="{ featured: packageData.is_featured }">
      <div v-if="packageData.is_featured" class="featured-box">
        <p>Preporučujemo</p>
      </div>
      <div class="heading-box">
        <h1>{{ packageData.name }}</h1>
      </div>
      <div class="separator"></div>

      <div class="box">
        <div>
          <img class="image-round" v-bind:src="imageTv" />
        </div>
        <div class="box-list">
          <ul>
            <li v-for="item in tv" :key="item.id">
              <div
                v-html="
                  boldString(item.long_name, item.attributes.attribute_value)
                "
              ></div>
            </li>
          </ul>
        </div>
      </div>

      <div class="separator"></div>

      <div class="box net">
        <div>
          <img class="image-round" v-bind:src="imageNet" />
        </div>
        <div class="box-list">
          <ul>
            <li v-for="item in net" :key="item.id">
              <div
                v-html="
                  boldString(item.long_name, item.attributes.attribute_value)
                "
              ></div>
            </li>
          </ul>
        </div>
      </div>

      <div
        v-if="
          packageData.promotions[0].discount_variations[0] === selectedContract
        "
        class="promo-box"
      >
        <div>
          <img v-bind:src="packageData.promotions[0].promotion_image" />
        </div>
        <div class="promo-text">
          <div v-html="packageData.promotions[0].promo_text"></div>
        </div>
      </div>

      <div class="separator"></div>

      <div class="price-box">
        <div v-if="priceRecurring" class="recurring">
          {{ priceRecurring | numeral("0.[0]") }} rsd/mes.
        </div>
        <div class="price-normal">{{ price | numeral("0,0") }} rsd/mes.</div>
      </div>
      <div
        class="promo-text-small"
        v-if="priceRecurring"
        v-html="packageData.prices.old_price_promo_text"
      ></div>
      <button>Naručite</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Paket",
  props: {
    packageData: Object,
    selectedContract: String,
    assets: Object,
  },
  methods: {
    boldString(str, substr) {
      var strRegExp = new RegExp(substr, "g");
      return str.replace(strRegExp, "<b>" + substr + "</b>");
    },
  },
  computed: {
    priceRecurring() {
      return this.packageData.prices.old_price_recurring[this.selectedContract];
    },
    price() {
      return this.packageData.prices.price_recurring[this.selectedContract];
    },
    imageNet() {
      return this.assets.net_category;
    },
    imageTv() {
      return this.assets.tv_category;
    },
    tv() {
      return this.packageData.included.filter(
        (included) => included.product_category === "tv"
      );
    },
    net() {
      return this.packageData.included.filter(
        (included) => included.product_category === "net"
      );
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.recurring {
  width: 50%;
  text-decoration: line-through;
  font-family: "Calibri";
  font-weight: normal;
  font-style: italic;
  font-size: 22px;
  color: #742d6c;
}
ul {
  padding-left: 0;
  li {
    list-style: none;
  }
}

.inner {
  background-color: #f8f4ec;
  display: flex;
  flex-direction: column;
  padding: 30px 20px;
  width: 100%;
  border-radius: 20px;
  position: relative;

  &.featured {
    border-top-left-radius: 0;
    border-top-right-radius: 0;
  }

  .featured-box {
    position: absolute;
    width: 100%;
    text-align: center;
    left: 0;
    top: -38px;
    background: #f8f4ec;
    font-family: "Calibri";
    font-weight: normal;
    font-style: italic;
    font-size: 14px;
    color: #742d6c;
    padding-top: 10px;
    padding-bottom: 10px;
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;

    &:after {
      content: "";
      position: absolute;
      left: 20px;
      bottom: 0;
      right: 20px;
      height: 1px;
      background-color: #ece9e4;
    }
  }

  .heading-box {
    height: 110px;
    display: flex;
    justify-content: center;

    h1 {
      align-self: center;
    }
  }
  .box {
    display: flex;
    align-items: center;
    min-height: 160px;

    &.net {
      min-height: 100px;
    }

    .box-list {
      text-align: left;
      margin-left: 50px;
      ul li {
        list-style-type: disc;
      }
    }
  }
}

.separator {
  background-color: #ece9e4;
  height: 1px;
  width: 100%;
  margin-top: 20px;
  margin-bottom: 20px;
}

.image-round {
  width: 45px;
}

.promo-box {
  display: flex;
  align-items: center;
  height: 80px;
  max-height: 80px;
  min-height: unset;
  background-color: white;

  img {
    width: 81px;
  }
  .promo-text {
    font-family: "Calibri";
    font-weight: bold;
    font-style: italic;
    font-size: 14px;
    color: #742d6c;
    margin-left: 20px;
  }
}

.price-box {
  display: flex;
  align-items: center;
  margin-top: 10px;
  margin-bottom: 30px;

  .price-normal {
    font-family: "Calibri";
    font-weight: bold;
    font-style: italic;
    font-size: 33px;
    color: #742d6c;
    margin: 0 auto;
  }
}

.promo-text-small {
  font-family: "Calibri";
  font-weight: normal;
  font-style: normal;
  font-size: 16px;
  color: #102542;
  margin-top: -10px;
  margin-bottom: 30px;
}

button {
  background-color: #742d6c;
  color: white;
  border-radius: 20px;
  transition: ease 1s all;
  padding: 12px;
  border: none;
  cursor: pointer;
  margin-top: auto;
  font-family: "Calibri";
  font-weight: bold;
  font-size: 18px;

  &:hover {
    opacity: 0.8;
  }
}
</style>
