<template>
  <q-page class="flex flex-center">
    <div class="row flex-center full-width">
      <div class="col-lg-8 col-xl-8 col-md-12 col-sm-12 col-xs-12">
        <q-form
          @submit="registerUser"
          ref="registrationForm"
        >
          <q-card class="col-12 q-ma-md shadow-15" square style="background:linear-gradient(to right, #1976D2 , #26A69A)">
            <q-card-section align="center" class="text-h5 text-weight-thin text-white bg-secondary">
              <div class="row justify-between">
                <div>
                  UERM ACADEMIC RECORDS APPLICATION
                </div>
                <q-btn
                  push
                  size="md"
                  :to="'/'"
                  icon="undo"
                  color="info"
                  label="GO BACK HOME"
                />
              </div>
            </q-card-section>
              <q-expansion-item
                expand-separator
                icon="group"
                label="BASIC INFORMATION"
                v-model="expansionItem1"
                group="primaryGroup"
                header-class="text-white"
                expand-icon-class="text-white"
                :disable="this.disableExpansion1"
              >
                <q-card-section>
                  <q-card class="q-my-md">
                    <q-card-section class="bg-secondary text-overline text-white" align="center">
                      PREVIOUS ACADEMIC RECORDS REQUEST
                    </q-card-section>
                    <q-card-section>
                      <div class="row justify-between q-col-gutter-lg">
                        <div class="" v-bind:class="{ 'col-lg-12 col-md-12 col-sm-12 col-xs-12 text-center': !registrationInfo.haveRecord }">
                          <q-toggle
                            v-model="registrationInfo.haveRecord"
                            checked-icon="check"
                            label="Do you have previous Academic Records Request?"
                            color="secondary"
                            unchecked-icon="clear"
                            class="q-mt-md q-pl-none"
                            size="xl"
                          />
                        </div>
                        <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12" v-if="registrationInfo.haveRecord">
                          <q-input
                            square
                            v-model="registrationInfo.referenceNumber"
                            type="text"
                            label="Reference Number"
                            color="secondary"
                            :rules="[ val => val && val.length > 0 || 'Please enter your Reference Number']"
                            @focusout="searchReferenceID(registrationInfo.referenceNumber)"
                          >
                            <template v-slot:prepend>
                              <q-icon name="group" />
                            </template>
                            <template v-slot:append>
                              <q-icon name="close" @click="registrationInfo.referenceNumber = ''" class="cursor-pointer" />
                            </template>
                          </q-input>
                        </div>
                        <div class="col-12" v-if="this.referenceNumberMessage !== null">
                          <q-banner inline-actions class="text-white bg-red">
                            {{ this.referenceNumberMessage }}
                          </q-banner>
                        </div>
                      </div>
                    </q-card-section>
                  </q-card>
                  <q-card>
                    <q-card-section class="bg-secondary" align="center">
                      <div class="text-overline text-white">
                        PERSONAL INFORMATION
                      </div>
                    </q-card-section>
                    <q-card-section>
                      <q-input
                        name="lastName"
                        ref="lastName"
                        square
                        v-model="registrationInfo.lastname"
                        type="text"
                        label="Lastname"
                        color="secondary"
                        :rules="[ val => val && val.length > 0 || 'Please enter your Lastname']"
                      >
                        <template v-slot:prepend>
                          <q-icon name="group" />
                        </template>
                        <template v-slot:append>
                          <q-icon name="close" @click="registrationInfo.lastname = ''" class="cursor-pointer" />
                        </template>
                      </q-input>
                      <q-input
                        name="firstName"
                        ref="firstName"
                        square
                        v-model="registrationInfo.firstname"
                        type="text"
                        label="Firstname"
                        color="secondary"
                        :rules="[ val => val && val.length > 0 || 'Please enter your Firstname']"
                      >
                        <template v-slot:prepend>
                          <q-icon name="group" />
                        </template>
                        <template v-slot:append>
                          <q-icon name="close" @click="registrationInfo.firstname = ''" class="cursor-pointer" />
                        </template>
                      </q-input>
                      <q-input
                        square
                        v-model="registrationInfo.middlename"
                        type="text"
                        label="Middlename"
                        color="secondary"
                        hint=""
                      >
                        <template v-slot:prepend>
                          <q-icon name="group" />
                        </template>
                        <template v-slot:append>
                          <q-icon name="close" @click="registrationInfo.middlename = ''" class="cursor-pointer" />
                        </template>
                      </q-input>
                      <q-input
                        type="email"
                        color="secondary"
                        ref="email"
                        v-model="registrationInfo.emailAddress"
                        label="Personal Email"
                        autocomplete="off"
                        :rules="[ val => val && val.length > 0 || 'Please enter your Email', isValidEmail]"
                      >
                        <template v-slot:prepend>
                          <q-icon name="group" />
                        </template>
                        <template v-slot:append>
                          <q-icon name="close" @click="registrationInfo.emailAddress = ''" class="cursor-pointer" />
                        </template>
                      </q-input>
                      <q-input
                        type="number"
                        color="secondary"
                        ref="mobileNumber"
                        name="mobileNumber"
                        v-model="registrationInfo.mobileNumber"
                        label="Your Contact Number"
                        autocomplete="off"
                        hint="Your Contact Number (09*********)"
                        :rules="[ val => val && val.length > 0 || 'Please enter your Contact Number', val => val.length <= 11 || 'Please enter valid contact number']"
                      >
                        <template v-slot:prepend>
                          <q-icon name="group" />
                        </template>
                        <template v-slot:append>
                          <q-icon name="close" @click="registrationInfo.mobileNumber = ''" class="cursor-pointer" />
                        </template>
                      </q-input>
                      <q-input
                        square
                        v-model="registrationInfo.address"
                        name="address"
                        ref="address"
                        type="text"
                        label="Address"
                        color="secondary"
                        :rules="[ val => val && val.length > 0 || 'Please enter your Address']"
                        hint=""
                      >
                        <template v-slot:prepend>
                          <q-icon name="group" />
                        </template>
                        <template v-slot:append>
                          <q-icon name="close" @click="registrationInfo.firstname = ''" class="cursor-pointer" />
                        </template>
                      </q-input>
                    </q-card-section>
                    <q-card-section class="bg-secondary" align="center">
                      <q-btn-group>
                        <q-btn
                          push
                          size="lg"
                          color="positive"
                          icon="arrow_forward"
                          :class="`full-height text-white`"
                          label="CONTINUE"
                          @click="continueExpansion('expansionItem2')"
                        />
                      </q-btn-group>
                    </q-card-section>
                  </q-card> 
                </q-card-section>
              </q-expansion-item>

              <q-expansion-item
                expand-separator
                icon="school"
                label="ACADEMIC INFORMATION"
                v-model="expansionItem2"
                group="primaryGroup" 
                header-class="text-white"
                expand-icon-class="text-white"
                :disable="this.disableExpansion2"
              >
                <q-card-section v-if="expansionItem2">
                  <q-card>
                    <q-card-section class="bg-secondary" align="center">
                      <div class="text-overline text-white">
                        STUDENT INFORMATION
                      </div>
                    </q-card-section>
                    <q-card-section>
                      <div class="row justify-between q-col-gutter-lg">
                        <div class="" v-bind:class="{ 'col-lg-12 col-md-12 col-sm-12 col-xs-12 text-center': registrationInfo.currentlyEnrolled }">
                          <q-toggle
                            v-model="registrationInfo.currentlyEnrolled"
                            checked-icon="check"
                            label="Currently Enrolled?"
                            color="secondary"
                            unchecked-icon="clear"
                            class="q-mt-md q-pl-none"
                            size="xl"
                          />
                        </div>
                        <div class="col-lg-6 col-md-6 col-sm-6 col-xs-12" v-if="!registrationInfo.currentlyEnrolled">
                          <q-input
                            name="dateOfGraduation"
                            v-model="registrationInfo.dateOfGraduation"
                            mask="date"
                            :rules="['date']"
                            label="Date of Graduation"
                            hint="(YYYY/MM/DD)"
                            autocomplete="off"
                            color="secondary"
                          >
                            <template v-slot:append>
                              <q-icon name="event" class="cursor-pointer">
                                <q-popup-proxy ref="qDateProxy" transition-show="scale" transition-hide="scale">
                                  <q-date v-model="registrationInfo.dateOfGraduation" color="secondary">
                                    <div class="row items-center justify-end">
                                      <q-btn v-close-popup label="Close" color="primary" flat />
                                    </div>
                                  </q-date>
                                </q-popup-proxy>
                              </q-icon>
                            </template>
                            <template v-slot:prepend>
                              <q-icon name="school" />
                            </template>
                          </q-input>
                        </div>
                        <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12 q-pt-sm">
                          <q-input
                            square
                            v-model="registrationInfo.studentNumber"
                            type="text"
                            label="Student #"
                            color="secondary"
                            hint="Student # is not required"
                            name="studentNo"
                          >
                            <template v-slot:prepend>
                              <q-icon name="school" />
                            </template>
                            <template v-slot:append>
                              <q-icon name="close" @click="registrationInfo.studentNumber = ''" class="cursor-pointer" />
                            </template>
                          </q-input>
                        </div>
                        <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12 q-pt-sm">
                          <q-input
                            name="degreeProgram"
                            square
                            v-model="registrationInfo.degreeProgram"
                            type="text"
                            label="Degree Program"
                            color="secondary"
                            hint=""
                            :rules="[ val => val && val.length > 0 || 'Please enter your Degree Program']"
                          >
                            <template v-slot:prepend>
                              <q-icon name="school" />
                            </template>
                            <template v-slot:append>
                              <q-icon name="close" @click="registrationInfo.degreeProgram = ''" class="cursor-pointer" />
                            </template>
                          </q-input>
                        </div>
                        <div class="col-lg-12 col-md-6 col-sm-12 col-xs-12 q-pt-sm">
                          <q-card>
                            <q-card-section class="bg-secondary" align="center">
                              <div class="text-overline text-white">
                                PERIOD ENROLLED IN THE MEDICAL CENTER FROM:
                              </div>
                            </q-card-section>
                            <q-card-section>
                              <div class="row justify-between q-col-gutter-lg">
                                <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12 q-pt-sm">
                                  <q-select
                                    name="periodEnrolledFromSchoolYear"
                                    v-model="registrationInfo.periodEnrolledFromSchoolYear"
                                    :options="schoolYearFrom"
                                    label="Period Enrolled S.Y. From"
                                    hint=""
                                    color="secondary"
                                    :rules="[ val => val && val.length > 0 || 'Field Required']"
                                  />
                                </div>
                                <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12 q-pt-sm">
                                  <q-select
                                    name="periodEnrolledfromTerm"
                                    v-model="registrationInfo.periodEnrolledfromTerm"
                                    :options="fromTerm"
                                    label="Term"
                                    hint=""
                                    color="secondary"
                                    :rules="[ val => val && val.length > 0 || 'Field Required']"
                                  />
                                </div>
                              </div>
                            </q-card-section>
                          </q-card>
                        </div>
                        <div class="col-lg-12 col-md-6 col-sm-12 col-xs-12 q-pt-sm">
                          <q-card>
                            <q-card-section class="bg-secondary" align="center">
                              <div class="text-overline text-white">
                                PERIOD ENROLLED IN THE MEDICAL CENTER TO:
                              </div>
                            </q-card-section>
                            <q-card-section>
                              <div class="row justify-between q-col-gutter-lg">
                                <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12 q-pt-sm">
                                  <q-select
                                    name="periodEnrolledToSchoolYear"
                                    v-model="registrationInfo.periodEnrolledToSchoolYear"
                                    :options="schoolYearTo"
                                    label="Period Enrolled S.Y. To"
                                    hint=""
                                    color="secondary"
                                    :rules="[ val => val && val.length > 0 || 'Field Required']"
                                  />
                                </div>
                                <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12 q-pt-sm">
                                  <q-select
                                    name="periodEnrolledToTerm"
                                    v-model="registrationInfo.periodEnrolledToTerm"
                                    :options="toTerm"
                                    label="Term"
                                    hint=""
                                    color="secondary"
                                    :rules="[ val => val && val.length > 0 || 'Field Required']"
                                  />
                                </div>
                              </div>
                            </q-card-section>
                          </q-card>
                        </div>
                      </div>  
                    </q-card-section>
                  </q-card>
                </q-card-section>
                <q-card-section class="bg-secondary" align="center">
                  <q-btn-group>
                    <q-btn
                      push
                      color="negative"
                      size="lg"
                      icon="arrow_back"
                      :class="`full-height text-white`"
                      label="GO BACK"
                      @click="backwardExpansion('expansionItem1')"
                    />
                    <q-btn
                      push
                      size="lg"
                      color="positive"
                      icon="arrow_forward"
                      :class="`full-height text-white`"
                      label="CONTINUE"
                      @click="continueExpansion('expansionItem3')"
                    />
                  </q-btn-group>
                </q-card-section>
              </q-expansion-item>

              <q-expansion-item
                expand-separator
                icon="folder_open"
                label="DOCUMENT REQUESTS"
                v-model="expansionItem3"
                group="primaryGroup"
                header-class="text-white"
                expand-icon-class="text-white"
                :disable="this.disableExpansion3"
              >
                <q-card-section v-if="expansionItem3">
                  <q-card>
                    <q-card-section class="bg-secondary text-overline text-white" align="center">
                      REQUEST FOR ACADEMIC RECORDS
                    </q-card-section>
                    <q-card-section align="right">
                      <q-btn
                        color="secondary"
                        @click="addDocumentRequest"
                        icon="add_circle_outline"
                        label="Add Academic Record Document"
                      >
                        <q-badge color="orange" floating>
                          {{ academicDocumentRequest.length }}
                        </q-badge>
                      </q-btn>
                    </q-card-section>
                    <q-card-section>
                      <div class="" 
                        v-for="(docRequest, index) in academicDocumentRequest"
                        :key="index"
                      >
                        <div v-if="index%2">
                          <q-card class="q-mb-sm">
                            <q-card-section class="bg-secondary text-white text-overline" align="center">
                              DOCUMENT #{{ index+1 }}
                            </q-card-section>
                            <q-card-section>
                              <div class="row justify-center q-col-gutter-md">
                                <div class="col-lg-7 flex flex-center">
                                  <q-select
                                    v-model="docRequest.document"
                                    :options="academicDocument"
                                    label="Document"
                                    hint="Select your requesting document"
                                    color="secondary"
                                    style="width:500px;"
                                    :rules="[ val => val && val.length > 0 || 'Please select document from the list']"
                                  />
                                </div>
                                <div class="col-lg-3 flex flex-center">
                                  <q-input
                                    v-model="docRequest.numberOfCopies"
                                    type="number"
                                    label="Number of Copies"
                                    color="secondary"
                                    :rules="[ val => val && val.length > 0 || 'Please enter Number of Copies']"
                                  >
                                    <template v-slot:prepend>
                                      <q-icon name="school" />
                                    </template>
                                    <template v-slot:append>
                                      <q-icon name="close" @click="docRequest.numberOfCopies = ''" class="cursor-pointer" />
                                    </template>
                                  </q-input>
                                </div>
                                <div class="col-lg-2 flex flex-center">
                                  <q-btn color="negative" @click="removeDocuments(index)" icon="delete_outline" />
                                </div>
                              </div>
                            </q-card-section>
                          </q-card>
                        </div>
                        <div v-else>
                          <q-card class="q-mb-sm">
                            <q-card-section class="bg-primary text-white text-overline" align="center">
                              DOCUMENT #{{ index+1 }}
                            </q-card-section>
                            <q-card-section align="center">
                              <div class="row justify-center q-col-gutter-md">
                                <div class="col-lg-7 flex flex-center">
                                  <q-select
                                    v-model="docRequest.document"
                                    :options="academicDocument"
                                    label="Document"
                                    hint="Select your requesting document"
                                    color="secondary"
                                    style="width:500px;"
                                    :rules="[ val => val && val.length > 0 || 'Please select document from the list']"
                                  />
                                </div>
                                <div class="col-lg-3 flex flex-center">
                                  <q-input
                                    square
                                    v-model="docRequest.numberOfCopies"
                                    type="number"
                                    label="Number of Copies"
                                    color="secondary"
                                    :rules="[ val => val && val.length > 0 || 'Please enter Number of Copies']"
                                  >
                                    <template v-slot:prepend>
                                      <q-icon name="school" />
                                    </template>
                                    <template v-slot:append>
                                      <q-icon name="close" @click="docRequest.numberOfCopies = ''" class="cursor-pointer" />
                                    </template>
                                  </q-input>
                                </div>
                                <div class="col-lg-2 flex flex-center">
                                  <q-btn color="negative" @click="removeDocuments(index)" icon="delete_outline" />
                                </div>
                              </div>
                            </q-card-section>
                          </q-card>
                        </div>
                      </div>
                    </q-card-section>
                    <q-card-section class="bg-secondary" align="center">
                      <q-btn-group>
                        <q-btn
                          push
                          color="negative"
                          size="lg"
                          icon="arrow_back"
                          :class="`full-height text-white`"
                          label="GO BACK"
                          @click="backwardExpansion('expansionItem2')"
                        />
                        <q-btn
                          push
                          size="lg"
                          color="positive"
                          icon="arrow_forward"
                          :class="`full-height text-white`"
                          label="CONTINUE"
                          @click="continueExpansion('expansionItem4')"
                        />
                      </q-btn-group>
                    </q-card-section>
                  </q-card>
                </q-card-section>
              </q-expansion-item>

              <q-expansion-item
                expand-separator
                icon="verified_user"
                label="VERIFICATION"
                v-model="expansionItem4"
                group="primaryGroup"
                header-class="text-white"
                expand-icon-class="text-white"
                :disable="this.disableExpansion4"
              >
                <q-card-section v-if="expansionItem4">
                  <q-card>
                    <q-card-section class="bg-secondary text-overline text-white" align="center">
                      STUDENT VERIFICATION
                    </q-card-section>
                    <q-card-section>
                      <div class="row">
                        <div class="col-12">
                          <q-file
                            name="student_file"
                            v-model="registrationInfo.fileProof"
                            label="File Proof"
                            color="secondary"
                            accept=".pdf"
                            hint="Upload a PDF File containing proofs that you are a Student of UERM"
                            :rules="[ val => val !== '' && val !== null || 'Please upload a proof that you are a former or a current student']"
                          >
                            <template v-slot:prepend>
                              <q-icon name="verified_user" />
                            </template>
                            <template v-slot:append>
                              <q-icon name="close" @click="registrationInfo.fileProof = ''" class="cursor-pointer" />
                            </template>
                          </q-file>
                        </div>
                      </div>
                    </q-card-section>
                    <q-card-section class="bg-secondary" align="center">
                      <q-btn-group>
                        <q-btn
                          push
                          color="negative"
                          size="lg"
                          icon="arrow_back"
                          :class="`full-height text-white`"
                          label="GO BACK"
                          @click="backwardExpansion('expansionItem3')"
                        />
                        <q-btn
                          push
                          size="lg"
                          color="positive"
                          icon="arrow_forward"
                          :class="`full-height text-white`"
                          label="CONTINUE"
                          @click="continueExpansion('expansionItem5')"
                        />
                      </q-btn-group>
                    </q-card-section>
                  </q-card>
                </q-card-section>
              </q-expansion-item>

              <q-expansion-item
                expand-separator
                icon="mode_edit_outline"
                label="TERMS AND CONDITIONS"
                v-model="expansionItem5"
                group="primaryGroup"
                header-class="text-white"
                expand-icon-class="text-white"
                :disable="this.disableExpansion5"
              >
                <q-card-section v-if="expansionItem5">
                  <q-card>
                    <q-card-section>
                      <div class="text-body1">
                        <q-checkbox
                          v-model="checkTerms"
                          :label="terms"
                          size="xl"
                          color="secondary"
                        >
                        </q-checkbox>
                      </div>
                    </q-card-section>
                    <q-card-section class="bg-secondary" align="center">
                      <q-btn-group>
                        <q-btn
                          push
                          color="negative"
                          size="lg"
                          icon="arrow_back"
                          :class="`full-height text-white`"
                          label="GO BACK"
                          @click="backwardExpansion('expansionItem4')"
                        />
                        <q-btn
                          push
                          size="lg"
                          color="positive"
                          icon="arrow_forward"
                          :class="`full-height text-white`"
                          label="SUBMIT"
                          type="submit"
                        />
                        <q-dialog v-model="confirmationDialog" persistent transition-show="flip-down" transition-hide="flip-up">
                          <q-card>
                            <q-card-section class="bg-secondary text-center text-h5 text-white">
                              <div class="row justify-center">
                                <div class="text-left text-h5 text-white text-weight-thin">
                                  CONFIRM ACADEMIC REQUEST
                                </div>
                              </div>
                            </q-card-section>
                            <q-card-section align="center" class="text-h5 text-weight-thin">
                              ARE YOU SURE YOU WANT TO SUBMIT THIS REQUEST?
                            </q-card-section>
                            <q-card-section class="bg-secondary" align="center">
                              <q-btn-group>
                                <q-btn
                                  push
                                  color="negative"
                                  size="lg"
                                  icon="arrow_back"
                                  :class="`full-height text-white`"
                                  label="GO BACK"
                                  @click="confirmationDialog = false"
                                />
                                <q-btn
                                  push
                                  size="lg"
                                  color="positive"
                                  icon="arrow_forward"
                                  :class="`full-height text-white`"
                                  type="submit"
                                  label="SUBMIT"
                                  @click.prevent="registerUser($event)"
                                />
                              </q-btn-group>
                            </q-card-section>
                          </q-card>
                        </q-dialog>
                      </q-btn-group>
                    </q-card-section>
                  </q-card>
                </q-card-section>
              </q-expansion-item>
            <q-card-section class="bg-secondary">
              <q-btn
                flat
                size="lg"
                :to="'/'"
                icon="undo"
                :class="`full-width full-height text-white`"
                label="GO BACK HOME"
              />
            </q-card-section>
          </q-card>
        </q-form>
      </div>
    </div>
  </q-page>
</template>

<script>
var term = [
  '1ST',
  '2ND',
  '3RD',
  'SUMMER'
]

var reasonForRequest = [
  'SCHOLARSHIP',
  'RE-ADMISSION',
  'TRANSFER TO OTHER SCHOOL',
  'BOARD EXAMINATION',
  'EMPLOYMENT',
  'FURTHER STUDIES',
  'REFERENCE',
  'OTHERS'
]

var academicRecords = [
  'OFFICIAL TRANSCRIPT OF RECORDS',
  'CERTIFICATE OF TRANSFER CREDENTIAL',
  'ORIGINAL DIPLOMA (ISSUED ONLY ONCE)',
  'CERTIFIED TRUE COPY - TRANSCRIPT OF RECORDS',
  'CERTIFIED TRUE COPY - DIPLOMA',
  'CERTIFICATION - GRADUATION',
  'CERTIFICATION - MEDIUM OF INSTRUCTION',
  'CERTIFICATION - ENROLLMENT/ATTENDANCE',
  'CERTIFICATION - NON-ISSUANCE OF SPECIAL ORDER NUMBER',
  'CERTIFICATION - GRADES',
  'CERTIFICATION - GENERAL WEIGHTED AVERAGE',
  'CERTIFICATION - COURSE / SUBJECT DESCRIPTION',
  'CERTIFICATION, AUTHENTICATION AND VERIFICATION - DEPARTMENT OF FOREIGN AFFAIRS (DFA)',
  'CERTIFICATION, AUTHENTICATION AND VERIFICATION - COMMISION ON HIGHER EDUCATION (CHED)',
  'OTHERS'
]
import { mapGetters } from 'vuex'
export default {
  name: 'AcademicRecordsApplication',
  data () {
    return {
      name: 'Jane Doe',
      submitResult: [],
      step: 1,
      schoolYearFrom: null,
      schoolYearTo: null,
      fromTerm: term,
      toTerm: term,
      reason: reasonForRequest,
      academicDocument: academicRecords,
      checkTerms: true,
      expansionItem1: true,
      expansionItem2: false,
      expansionItem3: false,
      expansionItem4: false,
      expansionItem5: false,
      disableExpansion1: false,
      disableExpansion2: true,
      disableExpansion3: true,
      disableExpansion4: true,
      disableExpansion5: true,
      confirmationDialog: false,
      terms: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      academicDocumentRequest: [
        {
          document: null,
          numberOfCopies: null
        }
      ],
      referenceNumberMessage: null,
      showOtherFileUploads: false,
      registrationInfo: {
        haveRecord: false,
        referenceNumber: null,
        // firstname: 'Bernard',
        // middlename: 'Tiaga',
        // lastname: 'Gresola',
        // address: 'Quezon City',
        // emailAddress: 'gresolabernard@gmail.com',
        // mobileNumber: '09053254071',
        firstname: null,
        middlename: null,
        lastname: null,
        address: null,
        emailAddress: null,
        mobileNumber: null,
        studentNumber: null,
        currentlyEnrolled: false,
        degreeProgram: null,
        dateOfGraduation: null,
        periodEnrolledFromSchoolYear: null,
        periodEnrolledfromTerm: null,
        periodEnrolledToSchoolYear: null,
        periodEnrolledToTerm: null,
        fileProof: null,
        otherFileUploads: null,
        reasonForRequest: null,
        otherReasons: null,
        academicDocument: null,
        otherDocuments: null
      }
    }
  },
  watch: {
    registrationInfo: {
      async handler (val) {
        // console.log(val)
      },
      deep: true
    },
    academicDocumentRequest: {
      async handler (val) {
        for (var result of val) {
          console.log(result)
          if (result.document !== null) {
            if (result.document.includes('CERTIFIED TRUE COPY')) {
              this.showOtherFileUploads = true
            } else {
              this.showOtherFileUploads = false
            }
          }
        }
      },
      deep: true
    }
    // step (val) {
    //   this.$refs.registrationForm.validate().then(async valid => {
    //     console.log(val)
    //     val = val - 1
    //     console.log(val)
    //   })
    // }
  },
  computed: {
    ...mapGetters({
      transactionDetails: 'students/transactionDetails'
    })
  },
  mounted () {
    this.formatBatch()
  },
  methods: {
    openConfirmationDialog () {
      this.confirmationDialog = true
    },
    submitConfirmation () {
      this.$refs.registrationForm.submit()
    },
    addDocumentRequest () {
      var newDocument = {
        document: null,
        numberOfCopies: null
      }

      this.academicDocumentRequest.push(newDocument)
    },
    removeDocuments (index) {
      const shallowCopyArray = Array.from(this.academicDocumentRequest)
      shallowCopyArray.splice(index, 1)
      this.academicDocumentRequest = shallowCopyArray
    },
    async registerUser (evt) {

      console.log(evt)
      if (confirm("ARE YOU SURE WANT TO SUBMIT THIS REQUEST?")) {
        const action = evt.target.offsetParent.form
        console.log(action, 'event')
        const formData = new FormData(evt.target)
          // console.log(formData)
          for (const [ name, value ] of formData.entries()) {
            console.log(name, value)
          }
        // if (this.checkTerms !== true) {
        //   this.$q.notify({
        //     color: 'red-5',
        //     textColor: 'white',
        //     icon: 'warning',
        //     message: 'You need to accept the terms and conditions first'
        //   })
        // } else {
        //   const formData = new FormData(evt.target)
        //   // console.log(formData)
        //   for (const [ name, value ] of formData.entries()) {
        //     console.log(name, value)
        //     // if (value.name.length > 0) {
        //     //   console.log(name)
        //     //   console.log(value.name)
        //     // }
        //   }

        //   // formData.append('lastname', this.registrationInfo.lastname)
        //   console.log(formData)
        //   // await this.$store.dispatch('students/registerUser', formData)
        // }
      }
    },
    isValidEmail (val) {
      const emailPattern = /^(?=[a-zA-Z0-9@._%+-]{6,254}$)[a-zA-Z0-9._%+-]{1,64}@(?:[a-zA-Z0-9-]{1,63}\.){1,8}[a-zA-Z]{2,63}$/
      return emailPattern.test(val) || 'Invalid email'
    },
    forwardStep (step) {
      this.$refs.registrationForm.validate().then(async valid => {
        if (!valid) {
          return
        } else {
          this.step = step
        }
      })
    },
    formatBatch () {
      var currentYear = new Date().getFullYear() + 1
      var batch = []
      for (var yearIndex = 1950; yearIndex <= currentYear; yearIndex++) {
        batch.push(yearIndex)
      }
      this.schoolYearTo = batch
      this.schoolYearFrom = batch
    },
    backwardStep (step) {
      this.step = step
    },
    continueExpansion (expansion) {
      this.$refs.registrationForm.validate().then(async valid => {
        if (!valid) {
          return
        } else {
          if (expansion === 'expansionItem1') {
            this.expansionItem1 = true
            this.expansionItem2 = false
            this.disableExpansion2 = true
          } else if (expansion === 'expansionItem2') {
            this.expansionItem2 = true
            this.disableExpansion2 = false
            this.expansionItem1 = false
            this.disableExpansion1 = true
          } else if (expansion === 'expansionItem3') {
            this.expansionItem3 = true
            this.disableExpansion3 = false
            this.expansionItem2 = false
            this.disableExpansion2 = true
          } else if (expansion === 'expansionItem4') {
            this.expansionItem4 = true
            this.disableExpansion4 = false
            this.expansionItem3 = false
            this.disableExpansion3 = true
          } else if (expansion === 'expansionItem5') {
            this.expansionItem5 = true
            this.disableExpansion5 = false
            this.expansionItem4 = false
            this.disableExpansion4 = true
          }
        }
      })
    },
    backwardExpansion (expansion) {
      console.log(expansion, 'expansion')
      if (expansion === 'expansionItem1') {
        this.expansionItem1 = true
        this.expansionItem2 = false
        this.disableExpansion2 = true
        this.disableExpansion1 = false
      } else if (expansion === 'expansionItem2') {
        this.expansionItem2 = true
        this.disableExpansion2 = false
        this.expansionItem3 = false
        this.disableExpansion3 = true
      } else if (expansion === 'expansionItem3') {
        this.expansionItem3 = true
        this.disableExpansion3 = false
        this.expansionItem4 = false
        this.disableExpansion4 = true
      } else if (expansion === 'expansionItem4') {
        this.expansionItem4 = true
        this.disableExpansion4 = false
        this.expansionItem5 = false
        this.disableExpansion5 = true
      } else if (expansion === 'expansionItem5') {
        this.expansionItem5 = true
        this.disableExpansion5 = false
      }
    },
    async searchReferenceID (referenceID) {
      referenceID = referenceID === '' ? null : referenceID
      if (referenceID !== null) {
        const validate = await this.authenticate()
        if (validate) {
          var reference = {
            referenceID: referenceID
          }
          await this.$store.dispatch('students/transactions', reference)
          if (this.transactionDetails.length > 0) {
            this.referenceNumberMessage = null
            this.registrationInfo.firstname = this.transactionDetails[0].FirstName
            this.registrationInfo.middlename = this.transactionDetails[0].MiddleName
            this.registrationInfo.lastname = this.transactionDetails[0].LastName
            this.registrationInfo.address = this.transactionDetails[0].Address
            this.registrationInfo.emailAddress = this.transactionDetails[0].Email
            this.registrationInfo.mobileNumber = this.transactionDetails[0].ContactNumber
          } else {
            this.referenceNumberMessage = 'Sorry, Reference Number not found. Please enter your personal details below.'
            this.registrationInfo.firstname = null
            this.registrationInfo.middlename = null
            this.registrationInfo.lastname = null
            this.registrationInfo.address = null
            this.registrationInfo.emailAddress = null
            this.registrationInfo.mobileNumber = null
          }
        }
      }
      
    },
    async authenticate () {
      const checkAuth = await this.$store.dispatch('students/checkAuthentication')
      if (!checkAuth) {
        await this.$store.dispatch('students/authenticateAPI')
        return { validated: true }
      } else {
        const validate = await this.$store.dispatch('students/validateToken')
        if (validate.message === 'error') {
          await this.$store.dispatch('students/authenticateAPI')
          return { validated: true }
        } else {
          return { validated: true }
        }
      }
    }
  }
}
</script>

<style>
  .card-border-primary {
    border-top: none;
    border-bottom: none;
    border-style: solid;
    border-color: rgba(0,0,0,0.12);
    border-left-color: #1976d2 !important;
    border-right-color: #1976d2 !important;
  }

  .card-border-secondary {
    border-style: solid;
    border-color: rgba(0,0,0,0.12);
    border-left-color: #26a69a  !important;
    border-right-color: #26a69a  !important;
  }
</style>