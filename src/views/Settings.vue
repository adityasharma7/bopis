<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>{{ $t("Settings") }}</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-list>
        <!-- Select store -->
        <ion-item>
          <ion-icon :icon="storefrontOutline" slot="start" />
          <ion-label>{{$t("Store")}}</ion-label>
          <ion-select interface="popover" :value="currentFacility.facilityId" @ionChange="setFacility($event)">
            <ion-select-option v-for="facility in (userProfile ? userProfile.facilities : [])" :key="facility.facilityId" :value="facility.facilityId" >{{ facility.name }}</ion-select-option>
          </ion-select>
        </ion-item>
        <!-- OMS information -->
        <ion-item>
          <ion-icon :icon="codeWorkingOutline" slot="start"/>
          <ion-label>{{ $t("OMS") }}</ion-label>
          <ion-note slot="end">{{ instanceUrl }}</ion-note>
        </ion-item>
        <!-- Profile of user logged in -->
        <ion-item>
          <ion-icon :icon="personCircleOutline" slot="start" />
          <ion-label>{{ userProfile !== null ? userProfile.partyName : '' }}</ion-label>
          <ion-button slot="end" fill="outline" color="dark" @click="logout()">{{ $t("Logout") }}</ion-button>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonButton, IonContent, IonHeader, IonIcon, IonItem, IonLabel, IonList, IonNote, IonPage, IonSelect, IonSelectOption, IonTitle, IonToolbar } from '@ionic/vue';
import { defineComponent } from 'vue';
import { ellipsisVertical, personCircleOutline, storefrontOutline, codeWorkingOutline } from 'ionicons/icons'
import { mapGetters, useStore } from 'vuex';
import { useRouter } from 'vue-router';

export default defineComponent({
  name: 'Settings',
  components: {
    IonButton, 
    IonContent, 
    IonHeader, 
    IonIcon,
    IonItem, 
    IonLabel, 
    IonList,
    IonNote,
    IonPage, 
    IonSelect, 
    IonSelectOption,
    IonTitle, 
    IonToolbar
  },
  computed: {
    ...mapGetters({
      userProfile: 'user/getUserProfile',
      currentFacility: 'user/getCurrentFacility',
      instanceUrl: 'user/getInstanceUrl'
    })
  },
  methods: {
    setFacility (facility: any) {
      if (this.userProfile)
        this.store.dispatch('user/setFacility', {
          'facility': this.userProfile.facilities.find((fac: any) => fac.facilityId == facility['detail'].value)
        });
    },
    logout () {
      this.store.dispatch('user/logout').then(() => {
        this.router.push('/login');
      })
    }
  },
  setup () {
    const store = useStore();
    const router = useRouter();

    return {
      ellipsisVertical,
      personCircleOutline,
      router,
      store,
      storefrontOutline,
      codeWorkingOutline
    }
  }
});
</script>