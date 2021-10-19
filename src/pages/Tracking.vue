<template>
  <q-page class="flex flex-center">
    <div class="row justify-center q-ma-sm">
      <div class="col-lg-12 col-sm-12 col-md-12 col-xs-12">
        <q-card square class="shadow-15 q-mb-lg">
          <q-card-section align="center" class="text-h5 text-weight-thin text-white bg-secondary">
            TRACK {{ this.$route.params.id }}
          </q-card-section>
          <q-card-section>
            <q-stepper
              v-model="step"
              ref="stepper"
              color="primary"
              animated
            >
              <q-step
                :name="1"
                title="VERIFICATION"
                icon="rule_folder"
                active-icon="rule_folder"
                :done="step > 1"
                style="min-height: 200px;"
              >
                <q-card square>
                  <q-card-section align="center" class="text-weight-thin text-h3 text-white bg-secondary">
                    VERIFYING REQUEST
                  </q-card-section>
                  <q-card-section align="center">
                    <div class="text-body1">
                      REGISTRAR IS VERIFYING YOUR REQUEST
                    </div>
                  </q-card-section>
                </q-card>
              </q-step>
              <q-step
                :name="2"
                title="PROCESSING"
                icon="drive_file_move"
                active-icon="drive_file_move"
                :done="step > 2"
                style="min-height: 200px;"
              >
                <q-card square>
                  <q-card-section align="center" class="text-weight-thin text-h3 text-white bg-secondary">
                    PROCESSING REQUEST
                  </q-card-section>
                  <q-card-section align="center">
                    <div class="text-body1">
                      REGISTRAR IS PROCESSING YOUR REQUEST
                    </div>
                  </q-card-section>
                </q-card>
              </q-step>
              <q-step
                :name="3"
                title="IN-TRANSIT"
                icon="local_shipping"
                active-icon="local_shipping"
                style="min-height: 200px;"
                :done="step > 3"
              >
                <q-card square>
                  <q-card-section align="center" class="text-weight-thin text-h3 text-white bg-secondary">
                    DOCUMENTS IN-TRANSIT
                  </q-card-section>
                  <q-card-section align="center">
                    <div class="text-body1">
                      DOCUMENTS ARE IN-TRANSIT
                    </div>
                  </q-card-section>
                </q-card>
              </q-step>
              <q-step
                :name="4"
                title="DELIVERED"
                icon="local_post_office"
                style="min-height: 200px;"
                active-icon="done"
                done-icon="done"
                :done="step > 3"
              >
                <q-card square>
                  <q-card-section align="center" class="text-weight-thin text-h3 text-white bg-secondary">
                    DOCUMENTS DELIVERED
                  </q-card-section>
                  <q-card-section align="center">
                    <div class="text-body1">
                      DOCUMENTS DELIVERED
                    </div>
                  </q-card-section>
                </q-card>
              </q-step>
            </q-stepper>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
// import VueOtpBox from "vue-otp-box";
// import "vue-otp-box/dist/VueOtpBox.css";
export default {
  name: 'Main',
  data () {
    return {
      step: 1
    }
  },
  methods: {
  }
}
</script>
