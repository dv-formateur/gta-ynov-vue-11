<template>
  <div class="profile">
    <div class="uk-margin-left">
      <h2>Votre profile</h2>
      <div class="uk-column-1-2 uk-align-center">
        <p>{{ profileUser.first_name }}</p>
        <p>{{ profileUser.last_name }}</p>
        <p>{{ profileUser.email }}</p>
        <p>{{ profileUser.phone }}</p>
        <p>{{ profileUser.adresse }}</p>
        <p>{{ profileUser.code_postale }}</p>
        <p>{{ profileUser.date_de_naissance }}</p>
      </div>
    </div>

    <hr>

    <div class="uk-margin-left">
      <h2>Modification de vos informations</h2>
      <form class="uk-form-stacked" @submit.prevent="modifierUser">
        <div>
          <label class="uk-form-label">Votre nouvelle adresse</label>
          <div class="uk-form-controls">
            <input class="uk-input" v-model="newAdresse" placeholder="Nouvelle adresse" type="text">
          </div>
        </div>
        <div>
          <label class="uk-form-label">Votre nouveau code postale</label>
          <div class="uk-form-controls">
            <input
              class="uk-input"
              v-model="newCodePostale"
              placeholder="Nouveau code postale"
              type="text"
            >
          </div>
        </div>
        <div style="margin-bottom: 15px;">
          <label class="uk-form-label">Votre nouveau numéro de téléphone</label>
          <div class="uk-form-controls">
            <input
              class="uk-input"
              v-model="newPhoneNumber"
              placeholder="Nouveau numéro de téléphone"
              type="text"
            >
          </div>
        </div>

        <div>
          <button type="submit" class="uk-button uk-button-primary">Valider</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import UIkit from "uikit";
export default {
  name: "Profile",
  data() {
    return {
      profileUser: {},
      newAdresse: "",
      newCodePostale: "",
      newPhoneNumber: ""
    };
  },
  methods: {
    async modifierUser() {
      axios
        .put("https://gta-ynov-vuejs-api.herokuapp.com/user", {
          firstName: this.profileUser.first_name,
          lastName: this.profileUser.last_name,
          email: this.profileUser.email,
          phone:
            this.newPhoneNumber != ""
              ? this.newPhoneNumber
              : this.profileUser.phone,
          adresse:
            this.newAdresse != "" ? this.newAdresse : this.profileUser.adresse,
          codePostale:
            this.newCodePostale != ""
              ? this.newCodePostale
              : this.profileUser.codePostale,
          dateNaissance: this.profileUser.date_de_naissance,
          role: this.profileUser.role,
          id: this.profileUser.id
        })
        .then(res => {
          this.getUser(this.profileUser.id);
          if (res.status == 200) {
            UIkit.notification({
              message: "Modification du profil prise en compte",
              status: "success",
              pos: "top-right",
              timeout: 2000
            });
          } else {
            UIkit.notification({
              message: "Erreur lors de la modification du profile",
              status: "error",
              pos: "top-right",
              timeout: 2000
            });
          }
        });
    },
    getUser(id) {
      axios
        .get("https://gta-ynov-vuejs-api.herokuapp.com/user/" + id)
        .then(res => {
          console.log(res);
          localStorage.user = JSON.stringify(res.data);
        });
    }
  },
  beforeMount() {
    this.profileUser = JSON.parse(localStorage.getItem("user"));
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>
