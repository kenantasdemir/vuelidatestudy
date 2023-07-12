<template>
    <div class="container py-5 h-50">
        <h1 class="text-center display-4">Vuelidate ile form verilerinizi kontrol edin.</h1>

        <div class=" d-flex justify-content-evenly align-content-center flex-row mt-5">


            <div class="card p-4 h-50" style="width:500px">
                <form @submit.prevent="onSubmit">
                    <div class="mb-3">
                        <label class="form-label">Adınız</label>
                        <input type="text" class="form-control" v-model.trim="customForm.first_name"
                            :class="{ 'is-invalid': v$.customForm.first_name.$error }">
                        <div class="text-danger" v-for="error of v$.customForm.first_name.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>



                        <small class="text-danger" v-if="v$.customForm.first_name.required"> </small>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Soyadınız</label>
                        <input type="text" class="form-control" v-model.trim="customForm.last_name"
                            :class="{'is-invalid':v$.customForm.last_name.$error}">
                        <div class="text-danger" v-for="error of v$.customForm.last_name.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Email adresiniz</label>
                        <input type="email" class="form-control" v-model.trim="customForm.email"
                            :class="{'is-invalid':v$.customForm.email.$error}">
                        <div class="text-danger" v-for="error of v$.customForm.email.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Yaş</label>
                        <input type="number" step="1" class="form-control" v-model.trim="customForm.age"
                            :class="{'is-invalid':v$.customForm.age.$error}">
                        <div class="text-danger" v-for="error of v$.customForm.age.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>
                    </div>

                    <div class="mb-3">
                        <label class="form-label">Teknolojiler</label>
                        <!-- <input type="number" step="1" class="form-control" v-model.trim="customForm.age"
                            :class="{'is-invalid':v$.customForm.age.$error}"> -->

                            <select v-model="selectedTechnology" class="form-select form-select-sm" name="teKnolojiler" id="">
                                <option v-for="(tek,index) in technologies" :key="index" :value="tek"> {{ tek }} </option>
                            </select>
                        
                        <div class="text-danger" v-for="error of v$.customForm.technologies.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>
                    </div>



                    <div class="mb-3">
                        <label class="form-label">Başlama tarihi</label>
                        <input type="date" step="1" class="form-control" v-model.trim="customForm.startDate"
                            :class="{'is-invalid':v$.customForm.startDate.$error}">
                        <div class="text-danger" v-for="error of v$.customForm.startDate.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Bitiş tarihi</label>
                        <input type="date" class="form-control" v-model.trim="customForm.endDate"
                            :class="{'is-invalid':v$.customForm.endDate.$error}">
                        <div class="text-danger" v-for="error of v$.customForm.endDate.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Parola</label>
                        <input type="password" class="form-control" v-model.trim="customForm.password"
                            :class="{'is-invalid':v$.customForm.password.$error}">
                        <div class="text-danger" v-for="error of v$.customForm.password.$silentErrors" :key="error.$uid">
                            {{ error.$message }}
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Parolanızı onaylayın</label>
                        <input type="password" class="form-control" v-model.trim="customForm.confirmPassword"
                            :class="{'is-invalid':v$.customForm.confirmPassword.$error}">
                        <div class="text-danger" v-for="error of v$.customForm.confirmPassword.$silentErrors"
                            :key="error.$uid">

                            {{ error.$message }}
                        </div>
                    </div>
                    <div class="mb-3 text-center d-flex justify-content-evenly">

                        <button  :disabled="v$.customForm.$invalid" class="btn btn-success">Gönder</button>
                        <button class="btn btn-danger" type="reset">Resetle</button>
                    </div>
                    <hr />

                </form>
            </div>

            <div class="card p-5 " style="min-width:350px; max-width:500px">
                {{ v$ }}
            </div>
        </div>
    </div>
</template>


<script>
import useVuelidate from '@vuelidate/core'
import { required, email, sameAs, between, alpha, helpers, minLength, maxLength } from '@vuelidate/validators'


const dateBefore = (value) => {
    console.log(value)
    return new Date(value) > new Date()
}

export default {
    name: 'CustomForm',
    setup() {
        return { v$: useVuelidate() }
    },
    data() {
        return {
            customForm: {
                first_name: '',
                last_name: '',
                email: '',
                age: '',
                startDate: "ssdsdsd",
                endDate: '',
                password: '',
                confirmPassword: '',
                technologies:["VueJS","React","Angular","Java"],
                selectedTechnology:"VueJS"
            },
        }
    },
    validations() {
        return {
            customForm: {
                first_name: {
                    required: helpers.withMessage("Bu alan zorunludur.", required),
                    alpha: helpers.withMessage("Bu alan sayılar ve semboller içeremez", alpha),
                    minLength: helpers.withMessage("Bu alan minimum 2 karakter olmalıdır.", minLength(2)),
                    // maxLength: helpers.withMessage("Bu alan maksimum 15 karakter olmalıdır.",maxLength(15))
                    maxLength: helpers.withMessage(
                        ({
                            $invalid,
                            $params,
                            $model
                        }) => `Bu alan '${$model.length}' karakter içeriyor fakat ${$params.min} karakter içermelidir. Bu yüzden ${$invalid ? 'geçersiz' : 'valid'}`,
                        minLength(4)
                    )
                },
                last_name: {
                    required: helpers.withMessage("Bu alan zorunludur.", required),
                    alpha: helpers.withMessage("Bu alan sayılar ve semboller içeremez.", alpha),
                    minLength: helpers.withMessage("Bu alan en az 3 karakter olmalıdır.", minLength(3)),
                    maxLength: helpers.withMessage("Bu alan en fazla 20 karakter olabilir.", maxLength(15))
                },
                email: {
                    required: helpers.withMessage("Bu alan zorunludur..", required),
                    email: helpers.withMessage("Lütfen geçerli formatta bir email adresi giriniz.", email)

                },
                age: {
                    required: helpers.withMessage("Bu alan zorunludur.", required),
                    between: helpers.withMessage("Bu alanın değeri 18 ile 60 arasında olmalıdır.", between(18, 60)),
                    // between:between(18,60)
                },
                technologies:{
                    $each:{
                        value:{
                            required:helpers.withMessage("Bu alan zorunludur",required),
                            minLength:minLength(6)
                        }
                    }
                },
                startDate: {
                    required: helpers.withMessage("Bu alan zorunludur. ", required),
                    dateAfter: helpers.withMessage('Başlama tarihi bugünden sonra olmalıdır', dateBefore),
                },
                endDate: {
                    required: helpers.withMessage("Bu alan zorunludur.", required),
                    minValue: helpers.withMessage('Bitiş tarihi başlama tarihinden sonra olmalıdır.', value => {
                        console.log(value)
                        return new Date(value) > new Date(this.customForm.startDate)
                    }),

                },
                password: {
                    required: helpers.withMessage("Bu alan zorunludur", required),
                    minLength:helpers.withMessage(`Parola en az 8 karakter içermelidir.`,minLength(8)),
                    maxLength:helpers.withMessage(`parola en fazla 16 karakter olabilir.`,maxLength(16))


                },
                confirmPassword: {
                    required: helpers.withMessage("Bu alan zorunludur.", required),
                    confirmPassword: helpers.withMessage("Parolalar eşleşmiyor.", sameAs(this.customForm.password))
                },
            },

        }
    },
    

}
</script>