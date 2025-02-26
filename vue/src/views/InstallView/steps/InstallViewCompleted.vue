<template>
    <div class="install-view-completed">
        <h1>Finished</h1>
        <MintStatusBox type="success">
            Your MintHCM installation is ready. Go ahead and log-in as an administrator.
        </MintStatusBox>
        <div class="install-view-anonymous-data">
            <v-checkbox v-model="sendAnonymousData" color="secondary" label="Send anonymous data" hide-details />
            <v-tooltip
                location="top start"
                text="Your IP address will be stored anonymously in the database for analytical purposes."
            >
                <template v-slot:activator="{ props }">
                    <div class="install-view-anonymous-data-help">
                        <v-icon v-bind="props" icon="mdi-help" size="14" color="white" />
                    </div>
                </template>
            </v-tooltip>
        </div>
        <MintButton variant="primary" text="Go to login" style="width: 100%" @click="goToLogin" />
    </div>
</template>

<script setup lang="ts">
import MintButton from '@/components/MintButtons/MintButton.vue'
import MintStatusBox from '@/components/MintStatusBox.vue'
import axios from 'axios'
import { ref } from 'vue'

const sendAnonymousData = ref(false)

async function goToLogin() {
    if (sendAnonymousData.value === true) {
        await sendAnonData()
    }
    location.reload()
}

async function sendAnonData() {
    await axios.post(`api/anonData`)
    .catch(error => {
        console.error(error)
    })
}
</script>

<style scoped lang="scss">
.install-view-completed {
    display: flex;
    flex-direction: column;
    gap: 24px;
    justify-content: center;
    text-align: center;

    h1 {
        font-size: 24px;
    }

    .install-view-anonymous-data {
        display: flex;
        gap: 16px;
        align-items: center;
        width: fit-content;

        .install-view-anonymous-data-help {
            border-radius: 50%;
            background: #00000061;
            width: 20px;
            height: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    }
}
</style>
