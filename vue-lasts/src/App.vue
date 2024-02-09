<script setup>
  import { computed, onMounted, reactive, ref, watch } from 'vue';
  import { account, ID } from './lib/appwrite.js';
  import axios from 'axios'
  import { provide } from 'vue'
  import Header from './components/Header.vue'
  import CardList from './components/CardList.vue'
  import Drawer from './components/Drawer.vue'

  const items = ref([])

  const cart = ref([])

  const isCreateingOrder = ref(false)

  const drawerOpen = ref(false);

  const totalPrice = computed(
    () => cart.value.reduce((acc, item) => acc + item.price, 0)
  )


  const closeDrawer = () => {
    drawerOpen.value = false
  }

  const openDrawer = () => {
    drawerOpen.value = true
  }

  const popatOpen = ref(false);


  const filters = reactive({
    sortBy: 'title',
    searchQuary: '',
  });

  const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded = true
  }

  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  }

  const createOrder = async () => {
    try {
      isCreateingOrder.value = true
      const {data} = await axios.post(`https://58f2106ddde1ddfe.mokky.dev/orders`, {
        items: cart.value,
        totalPrice: totalPrice.value,
      })

      cart.value = []


      return data;
    }catch (err) {
      console.log(err)
    } finally {
      isCreateingOrder.value = false
    }
  }


  const onClickAddPlus = (item) => {
    if (!item.isAdded) {
      addToCart(item)
    } else {
      removeFromCart(item)
    }
  }

  const onChangeSelect = (event) => {
    filters.sortBy = event.target.value
  }

  const onChangeSearchInput = (event) => {
    filters.searchQuary = event.target.value
  }


  const fetchItems = async () => {
    try {

      const params = {
        sortBy: filters.sortBy,
      }

      if(filters.searchQuary) {
        params.title = `*${filters.searchQuary}*`;
      }


    const { data } = await axios.get('https://58f2106ddde1ddfe.mokky.dev/items', 
      {
        params
      }
    );

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false,
    }));
  }catch (err) {
    console.log(err);
  }
  }

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});
watch(filters, fetchItems)



provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})



const addLogins = (loggedInUser) => {
  console.log(loggedInUser)
  return loggedInUser ? `Logged in as ${loggedInUser.name}` : 'Not logged in' 
}

const loggedInUser = ref(null);
const email = ref('');
const password = ref('');
const name = ref('');

const login = async (email, password) => {
  await account.createEmailSession(email, password);
  loggedInUser.value = await account.get();
};

const register = async () => {
  await account.create(ID.unique(), email.value, password.value, name.value);
  login(email.value, password.value);
};

const logout = async () => {
  await account.deleteSession('current');
  loggedInUser.value = null;
};
</script>


<template>
  <div class="rod">
    <div class="headerse">
      <Header :addLogins="addLogins" :loggedInUser="loggedInUser" :total-price="totalPrice" @openDrawer="openDrawer"/>

      <li class="right__blocks">
        <img src="/profile.svg" alt="">
        <b @click="popatOpen = !popatOpen">Профиль.</b>
      </li>
    </div>
    <Drawer v-if="drawerOpen" :total-price="totalPrice" @create-order="createOrder" is-createing-order="isCreateingOrder"/>


    <div class="rod__card">
      <div>

          <div class="rod__form" v-if="popatOpen">
            <form class="logins__form">
              <input class="input__logins" type="email" placeholder="Email" v-model="email" />
              <input class="input__logins" type="password" placeholder="Password" v-model="password" />
              <input class="input__logins" type="text" placeholder="Name" v-model="name" />
              <div class="rod__buttons">
                <button class="buttons__logins" type="button" @click="login(email, password)">Login</button>
                <button class="buttons__logins" type="button" @click="register">Register</button>
                <button class="buttons__logins" type="button" @click="logout">Logout</button>
              </div>
            </form>
          </div>
        </div>

      <div class="dop">
        <h2 class="all__sneak">Все кроссовки</h2>

        <div>
            <select @change="onChangeSelect" class="select__option">
              <option value="name">По названию</option>
              <option value="price">По цене (дешевые)</option>
              <option value="-price">По цене (дорогие)</option>
            </select>

          <div>
            <input @input="onChangeSearchInput" class="input__sneaker" type="text" placeholder="Поиск...">
          </div>
        </div>

      </div>
      
    </div>

    <div class="mooky__sneak">
      <CardList :items="items" @add-to-cart="onClickAddPlus"/>
    </div>
  </div>
</template>


