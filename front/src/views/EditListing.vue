<template>
  <div class="app-wrapper">
    <div class="cover-img">
      <img src="https://picsum.photos/200/300" alt="">
    </div>
    <div
      v-if="user"
      class="edit"
    >
      <BackButton />
      <h2>Edit or Delete</h2>
      <div v-if="!listing.author">
        <h3>Error!</h3>
        <h4>Sorry this page isn't avalible, please check the link and try again</h4>
        <button>Return to Home</button>
      </div>
      <div v-else-if="user.id === listing.author._id">
        <form
          @submit.prevent="checkForm"
        >
          <div class="form-group">
            <label for="title">Title</label>
            <input
              v-model="listing.title"
              type="text"
              name="title"
            >
          </div>
          <span id="title-error"><p>Please Enter A Title</p></span>
          <div id="price-and-category">
            <div class="form-group price-group">
              <label for="price">Price</label>
              <div id="wrap-price">
                <input
                  id="price-sign"
                  type="text"
                  value="$"
                  readonly="readonly"
                >
                <input
                  id="price-input"
                  v-model="listing.price"
                  type="number"
                  name="price"
                  max="999"
                  min="1"
                >
                <input
                  id="price-static"
                  type="text"
                  value=".00"
                  readonly="readonly"
                >
              </div>
              <span id="price-error"><p>Please Enter A Valid Price</p></span>
            </div>
            <div
              id="category-selection"
              class="form-group"
            >
              <label for="category">Category</label>
              <select
                id="category"
                v-model="listing.category"
                name="category"
                placeholder="select"
              >
                <option value="Tea">
                  Tea
                </option>
                <option value="Teacups">
                  Teacups
                </option>
                <option value="Teapots">
                  Teapots
                </option>
                <option value="Tea Sets">
                  Tea Sets
                </option>
                <option value="Misc">
                  Misc
                </option>
              </select>
            </div>
          </div>
          <div class="form-group">
            <label for="description">Product Description</label>
            <textarea
              v-model="listing.description"
              maxlength="200"
              row="50"
              name="description"
            />
          </div>
          <span id="description-error"><p>Please Enter A Description</p></span>
          <div class="form-group">
            <label for="image-url">Image URL</label>
            <input
              v-model="listing.imageUrl"
              type="text"
              name="image-url"
              placeholder="eg: https://image-url"
            >
          </div>
          <span id="image-error"><p>Please Enter A Valid Image URL</p></span>
          <button type="submit">
            Save Changes
          </button>
          <Delete :listing-id="listingId" />
        </form>
      </div>
      <div v-else>
        <h3>Error!</h3>
        <h4>Sorry you don't have the authorization to view this page</h4>
        <button>
          <router-link
            :to="('/listings')"
            class="view-detail-btn"
          >
            Return to Home
          </router-link>
        </button>
      </div>
    </div>
    <UserErrorMessage v-else />
  </div>
</template>

<script>
// eslint-disable-next-line import/no-unresolved
import Delete from '../components/DeleteButton.vue';
// discussed with Simon - 14/09
import UserErrorMessage from '../components/UserErrorMessage.vue';
import BackButton from '../components/BackButton.vue';

export default {
  components: {
    Delete,
    UserErrorMessage,
    BackButton,
  },
  props: {
    listingId: String,
    user: Object,
  },
  data() {
    return {
      listing: [],
      isError: false,
    };
  },
  created() {
    this.getListingDetail();
  },
  methods: {
    async getListingDetail() {
      const response = await fetch(
        `http://localhost:3000/listings/${this.listingId}`,
      );
      const data = await response.json();
      this.listing = data;
    },
    checkForm() {
      this.isError = false;
      if (!this.listing.title) {
        document.getElementById('title-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('title-error').style.display = 'none';
      }
      if (!this.listing.price) {
        document.getElementById('price-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('price-error').style.display = 'none';
      }
      if (!this.listing.description) {
        document.getElementById('description-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('description-error').style.display = 'none';
      }
      if (!this.listing.imageUrl) {
        document.getElementById('image-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('image-error').style.display = 'none';
      }
      if (this.isError === false) {
        this.updateListing();
      }
    },
    async updateListing() {
      const response = await fetch(
        `http://localhost:3000/listings/edit/${this.listingId}`,
        {
          method: 'PATCH',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            title: this.listing.title,
            author: this.listing.author,
            price: this.listing.price,
            category: this.listing.category,
            imageUrl: this.listing.imageUrl,
            description: this.listing.description,
          }),
        },
      );
      const data = await response.json();
      console.log(data);
      window.location.assign(`/listings/${this.listingId}`);
    },
  },
};
</script>

<style scoped>
.app-wrapper{
  background-color: #F4F1E9;
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 105vh;
}
.edit{
  margin-top: 1em;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
h2{
  margin-bottom: 1.5em;
}
form {
  width: 80vw;
  height: 75vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  justify-content: space-between;
}

input,
textarea {
  width: 100%;
}

input,
textarea,
select {
  border: thin #a26360 solid;
  border-radius: 3px;
  padding: 0.6em 0.4em;
}

select {
  background-color: white;
  padding: 0.5em 0.4em;
}

textarea {
  height: 15vh;
  font-family: 'Questrial', sans-serif;
  resize: none;
}

input:focus, textarea:focus {
  outline: none;
}

#price-and-category {
  width: 76vw;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

#price-and-category input,
#price-and-category select {
  width: 9.8em;
}

.form-group {
  width: 93%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

label {
  margin-bottom: 0.3em;
}

button {
  background-color: #a9c596;
  color: #2b463c;
  padding: 0.8em 1.5em;
  border-radius: 5px;
  border: none;
  margin-bottom: 1.5em;
  /* margin-top: 0em; */
  cursor: pointer;
}

#wrap-price{
  display: flex;
}

#wrap-price input{
  width: 6vw;
  padding: 0.56em 0.1em;
  padding-left: 0.2em;
}

#wrap-price input:focus{
  outline: none;
}

#price-sign{
  border-right: hidden;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  margin-right: -1.2em;
  color: #2b463c77;
}

#price-input{
  border-right: hidden;
  border-left: hidden;
  border-radius: 0;
  margin-right: -0.1em;
}

#price-static{
  border-left: hidden;
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  color: #2b463c77;
}
#category-selection{
  margin-right: -2.5em;
}
#title-error, #price-error, #description-error, #image-error{
  display: none;
  color: red;
}
.cover-img{
  display: none;
}
h2{
  margin-top: 4em;
}
@media screen and (min-width: 768px) {
  .edit{
    font-size: 1.2em;
  }
  input{
    height: 3vh;
  }
  button{
    padding-bottom: 1.8em;
    font-size: 0.8em;
  }
}
@media screen and (min-width: 1024px) {
  .app-wrapper{
    width: 100vw;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
  .cover-img{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50vw;
    overflow: hidden;
  }
  .cover-img>img{
    margin-top: 5em;
    width: 40vw;
    height: 60vh;
    border-radius: 10px;
  }
  .price-group{
    /* border: red thin solid; */
    padding-left: 23.5%;
  }
  .edit{
    width: 50vw;
  }
  button{
    margin-top: 2em;
  }
  .form-group,h2{
    width: 50%;
  }
}
</style>
