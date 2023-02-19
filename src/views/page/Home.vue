<template>
  <div>
    <div class="card home_intro">
      <div class="card-content">
        <span class="card-title">Welcome to Fort Worth ARTCC</span>
        <p>
          The Fort Worth ARTCC covers parts of Texas, Oklahoma, New Mexico, Louisiana, and Arkansas, with a square mileage of over 147,000. Our goal and mission are to provide professional air traffic control services, foster a welcoming community, and enjoy this hobby to its greatest extent possible. Students get a unique experience here with our student: instructor assignment program and get the advantage of consistent instruction and scheduling once assigned an instructor. We hope to see you on the scope soon!
        </p>
      <br>
        <p>
          “Texas is neither southern nor western. Texas is Texas.”
        </p>
        <div class="visit" v-if="!user.isLoggedIn || (!user.data.vis && !user.data.isMem)">
          <p>
            If you would like to become a visitor, please click the button below.
          </p>
          <router-link to="/controllers/visit" class="btn btn-waves">Become a Visitor</router-link>
        </div>
      </div>
    </div>
    <EventSummary />
    <NewsSummary />
    <WeatherInfo />
  </div>
</template>

<script>
import { mapState } from 'vuex';
import WeatherInfo from '../partial/WeatherInfo.vue';
import NewsSummary from '../partial/NewsSummary.vue';
import EventSummary from '../partial/EventSummary.vue';

export default {
  name: 'Home',
  title: 'Home',
  components: {
    NewsSummary,
    WeatherInfo,
    EventSummary
  },
  computed: {
    ...mapState('user', [
      'user'
    ])
  }
};
</script>

<style scoped lang="scss">
.visit {
  margin-top: 1em;
  text-align: center;

  p {
    text-align: left;
  }

  .btn {
    margin-top: 1em;
  }
}
</style>