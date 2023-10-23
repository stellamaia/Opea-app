<template>
    <div class="company-register">
        <NavBar />

        <div class="container-input">
            <div class="input-content">

                <v-form ref="form" @submit.prevent="searchCompanies">
                    <input v-model="searchText" class="form-control" type="search" placeholder="Buscar empresa..."
                        aria-label="Search">
                    <button class="btn-search" type="submit">
                        <img class="img-search" src="@/assets/search.png" alt="Search Icon">
                    </button>
                </v-form>

            </div>


            <div class="add-company-btn">
                <div class="rounded-icon-btn">
                    <img class="list-icon" src="@/assets/list.png" alt="Add Icon">
                </div>
                <v-btn class="add-btn " @click="dialog = true" id="meuBotao">Adicionar Empresa</v-btn>
            </div>
            <div v-if="companies.length > 0">

                <div v-for="company in companies" :key="company.id" class="add-company-input">

                    <div @click="openModalCompany(company)" class="add-btn-input" id="meuBotao">
                        <div class="rounded-icon-input">
                            <img class="list-icon-input" src="@/assets/list-input.png" alt="Add Icon">
                        </div>
                        <div class="content-items">
                            <p class="item company">{{ company.name }}</p>
                            <div class="item-flex">
                                <p class="item item-cnpj">CNPJ: {{ company.cnpj }} - </p>


                                <p class="item item-company">Email: {{ company.email }}</p>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div v-else class="search-company">

                <p>Sem empresa para mostrar</p>
                <span class="material-symbols-outlined">
                    block
                </span>
            </div>

        </div>


        <v-dialog v-model="dialog" persistent max-width="390">
            <v-card>
                <div class="content-card">
                    <v-card-title class="title-modal">Cadastrar Empresa</v-card-title>
                    <v-btn class="close-btn">
                        <v-img class="close-modal-register" @click="closeDialog" src="../assets/close.png"></v-img>
                    </v-btn>
                </div>

                <v-divider class="divider"></v-divider>
                <v-card-text>
                    <v-form>
                        <v-row>
                            <v-col cols="12">
                                <label class="label">Nome</label>
                                <input class="input" type="text" id="name" v-model="nome" />
                            </v-col>
                            <v-col cols="12">
                                <label class="label label-cnpj">CNPJ</label>
                                <input class="input" type="text" id="cnpj" v-model="cnpj" v-mask="'##.###.###/####-##'" />
                            </v-col>
                            <v-col cols="12">
                                <label class="label">E-mail</label>
                                <input class="input" type="text" id="email" v-model="email" />
                            </v-col>
                            <v-col style="padding-bottom: 0;" cols="12">
                                <v-alert v-if="showAlert" class="alert" border="left" color="red" type="error">
                                    Preencha os campos
                                </v-alert>
                            </v-col>
                        </v-row>
                    </v-form>
                </v-card-text>
                <v-card-actions class="content-btn">

                    <v-btn color="delete-btn " @click="deleteCompany">
                        <img class="delete-btn-img" src="../assets/delete.png" />
                    </v-btn>

                    <div class="container-btn">
                        <v-btn class="btn-cancel" @click="closeDialog">Cancelar</v-btn>
                        <v-btn v-if="!id" class="btn-register" @click="createCompany">Cadastrar</v-btn>
                        <v-btn v-else class="btn-register-update" @click="updateCompany">Alterar</v-btn>
                    </div>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>
<script>
import NavBar from './NavBar.vue';
import axios from 'axios'

export default {
    name: 'CompanyRegister',
    components: {
        NavBar
    },

    data() {
        return {
            searchText: "",
            dialog: false,
            valid: true,
            id: '',
            nome: '',
            email: '',
            cnpj: '',
            showAlert: false, 
            preventRegistration: false, 
            companies: [],
           
           
        };
    },
    created() {
        this.getCompanies();
    },
    methods: {

        openDialog() {
            this.dialog = true;
        },
        closeDialog() {
            this.dialog = false;
            this.resetForm();
            this.resetAlert();

        },

        resetAlert() {
            this.showAlert = false; 
        },
        resetForm() {
            this.id = "";
            this.nome = "";
            this.cnpj = "";
            this.email = "";
            this.$refs.form.resetValidation();

        },
        searchCompanies() {
            axios.get(`https://outros.opea-uat.solutions/prova/front/api/clients?text=${this.searchText}`).then((response) => {
                this.companies = response.data;
            });
        },
        getCompanies() {
            axios.get(`https://outros.opea-uat.solutions/prova/front/api/clients`).then((response) => {
                this.companies = response.data;
            })
        },
        createCompany() {
            if (this.nome.trim() === '' || this.cnpj.trim() === '' || this.email.trim() === '') {
                this.showAlert = true; // Show the alert
                this.preventRegistration = true; // Prevent registration
            } else {
                this.showAlert = false; // Hide the alert
                this.preventRegistration = false; // Allow registration
                this.$refs.form.validate();
                if (this.valid) {
                    axios.post(`https://outros.opea-uat.solutions/prova/front/api/clients`, {
                        name: this.nome,
                        cnpj: this.cnpj,
                        email: this.email,
                    }).then((response) => {
                        this.companies.push(response.data);
                        this.closeDialog();
                    });
                }
            }
        },
        updateCompany() {
            if (this.nome.trim() === '' || this.cnpj.trim() === '' || this.email.trim() === '') {
                this.showAlert = true; // Show the alert
            }
            else {

                axios.put(`https://outros.opea-uat.solutions/prova/front/api/clients/${this.id}`, {
                    name: this.nome,
                    cnpj: this.cnpj,
                    email: this.email,
                }).then((response) => {
                    this.companies = this.companies.map(company => {
                        if (company.id === this.id) {
                            return response.data
                        } else {
                            return company
                        }
                    }
                    );
                    this.closeDialog();
                });

            }
        },
        deleteCompany() {
            const id = this.id;
            axios.delete(`https://outros.opea-uat.solutions/prova/front/api/clients/${id}`)
                .then(() => {
                    // Remova a empresa excluída da lista
                    this.companies = this.companies.filter(company => company.id !== id);
                    this.closeDialog();
                })
                .catch(error => {
                    console.error('Erro ao excluir a empresa:', error);
                });

        },
        openModalCompany(company) {
            this.id = company.id;
            this.nome = company.name;
            this.cnpj = company.cnpj;
            this.email = company.email;
            this.dialog = true;

        },
    }

}
</script>


<style scoped >
* {
    outline: none;
}

input:focus {
    outline: none;
}

.company-register {
    background-color: #F5F5F5;
    height: 100vh;
}

.container-input {
    padding: 50px 30px 50px 30px;
    background-color: #F5F5F5;
}

.v-card.v-sheet.theme--light {
    padding: 0px !important;
}

.form-control {
    width: 300px !important;
}

.input-content {
    display: flex;
    justify-content: end;
    padding-top: 50px;
}

.img-search {
    top: 180px;
    height: 20px;
    right: 40px;
    position: absolute;
}

.add-company-btn {
    padding-top: 40px;
    background-color: #F5F5F5 !important;
}

.add-company-input {
    padding-top: 10px;
    background-color: #F5F5F5 !important;
}

.add-btn {
    height: 50px !important;
    width: 100%;
    box-shadow: none;
    border: 1.3px solid rgb(217, 217, 217);
    background-color: #F5F5F5 !important;
    border-radius: 30px 6px 6px 30px;
    font-size: 13px !important;
    font-weight: bold !important;
    color: #969696 !important;
}

.add-btn-input {
    height: 50px !important;
    width: 100%;
    display: flex;
    box-shadow: none;

    background-color: #ffffff !important;
    border-radius: 30px 6px 6px 30px;
    font-size: 14.5px !important;
    font-weight: 500 !important;
    color: #616161 !important;
    padding-left: 8px;
    padding-top: 2px;
}

.content-items {
    padding-left: 15px;
    padding-top: 6px;
}

.item {
    font-size: 10px;
    margin: 0;

}

.item-cnpj {
    padding: 0 3px 0 0 !important;
}

.company {
    font-size: 12.5px !important;
    font-weight: 500 !important;
}

.item-flex {
    display: flex;
    padding-top: 1px;
}

.v-btn {
    text-transform: none;
}

.rounded-icon-btn {
    position: absolute;
    left: 39px;
    z-index: 1;
    height: 40px;
    width: 40px;
    border-radius: 50%;
    background-color: #F5F5F5;
    border: 1.2px solid rgb(142, 142, 142);
    display: flex;
    justify-content: center;
    align-items: center;
}

.rounded-icon-input {
    height: 40px;
    width: 40px;
    border-radius: 50%;
    background-color: #F5F5F5;
    border: 1.3px solid #949494;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 2px;
}

.list-icon {
    height: 25px;

}

.list-icon-input {
    height: 25px;

}

.title-modal {
    color: #969696;
    font-size: 15px !important;
    font-weight: 400;
}


.close-modal-register {
    height: 50px;
    width: 50px;
}

.content-card {
    justify-content: space-between;
    display: flex;
}

.label {
    padding-right: 20px;
    display: block;
    color: #630A37;
    font-weight: 500;
}

.label-cnpj {
    text-transform: uppercase;
}

.input {
    border: 1.4px solid rgb(226, 226, 226);
    border-radius: 3px;
    height: 40px;
    width: 100%;
    padding-left: 10px;
    padding-right: 10px;
}

.content-btn {
    padding: 24px !important;
    justify-content: space-between;
}

.delete-btn {
    background-color: white !important;
    box-shadow: none;
    border: 1.4px solid rgb(226, 226, 226);
    border-radius: 3px;
    height: 35px !important;
    min-width: 10px !important;
}

.delete-btn-img {
    height: 20px !important;
    min-width: 0 !important;
}

.btn-cancel {
    background-color: white !important;
    box-shadow: none;
    border: 1.4px solid rgb(226, 226, 226);
    border-radius: 5px;
    margin: 0 20px 0 0 !important;
}

.btn-cancel:hover {
    background-color: #b2556ebd !important;
    color: white;
    border-color: #b2556e00;
}

.btn-register,
.btn-register-update {
    background-color: #630A37 !important;
    color: white !important;
    box-shadow: none;
    border: none;
    border-radius: 5px;
    margin: 0 !important;

}

.theme--light.v-btn.v-btn--has-bg {
    background-color: transparent;
}

.btn-register:hover,
.btn-register-update:hover {
    background-color: #b2556ebd !important;
    border: none;
    color: white !important;

}

.divider {
    margin: 0;
    border: 1px solid;
    border-color: rgb(120 120 120 / 65%);
}

.close-btn {
    background-color: rgba(255, 255, 255, 0) !important;
    box-shadow: none;
    width: 0px;
    padding-top: 30px !important;
    margin-right: 10px !important;
}

.close-btn::before {
    background-color: transparent;
    height: 20px;
    margin-top: 20px;
}

.form-control:focus {

    background-color: transparent;
    border-color: rgb(226, 226, 226);
    outline: 0;
    box-shadow: none;
}

.search-company {
    text-align: center;
    font-size: 13px !important;
    font-weight: bold !important;
    color: #969696 !important;
    padding-top: 20px;
}

.alert {
    padding: 0 0 0px 11px;
    font-size: 10px;
    background-color: #b2556ebd !important;
    border: none;

}

@media screen and (max-width: 300px) {
    .input-content {
        display: flex;
        justify-content: center;

    }

    .form-control {
        width: 100% !important;
    }

    .img-search {
        position: relative;
        left: 177px !important;
        top: -31px;
    }

    .rounded-icon-btn {
        top: 286px !important;
        left: 19px;
    }

    .container-input {
        padding: 50px 10px 50px 10px;
    }

    .content-items {
        padding-top: 14px;
        padding-left: 5px;
    }

    .add-btn {
        padding-left: 36px !important;
    }

    .add-btn-input {
        height: 0;
        padding-top: 0;
    }

    .item {
        font-size: 6px !important;
    }

    .item-flex {
        padding-top: 0;
    }

    .item-cnpj {
        padding: 0 5px 0 0 !important;
    }

    .item-company {
        padding: 0 0 0 5px;
    }

    .rounded-icon-input {
        margin-top: 4px;
    }

    .title-modal {
        font-size: 70% !important;

    }

    .container-btn {
        display: flex !important;

    }

    .btn-cancel {
        padding: 5px !important;
        margin-right: 3px !important;
        font-size: 12px;
    }

    .btn-register {
        padding: 5px !important;
        margin-left: 3px !important;
        font-size: 12px;
    }

}

@media screen and (min-width: 300px) and (max-width: 354px) {
    .item {
        font-size: 8px !important;


    }

    .content-items {
        padding-top: 13px !important;
    }

}

@media screen and (min-width: 301px) and (max-width: 600px) {
    .input-content {
        display: flex;
        justify-content: center;

    }

    .img-search {
        left: 267px;
        position: relative;
        top: -31px;
    }

    .container-input {
        padding: 50px 10px 50px 10px;
    }

    .content-items {
        padding-top: 7px;
    }

    .add-btn-input {
        height: 0;
        padding-top: 0;
    }

    .item-flex {
        padding-top: 0;
    }

    .rounded-icon-input {
        margin-top: 4px;
    }

    .rounded-icon-btn {
        left: 19px;
        top: 261px !important;
    }

    .container-btn {
        display: flex !important;

    }

    .btn-cancel {
        padding: 5px !important;
        margin-right: 10px !important;
        font-size: 12px;
    }

    .btn-register {
        padding: 5px !important;
        margin-left: 3px !important;
        font-size: 12px;
    }
}

@media screen and (min-width: 601px) {
    .img-search {
        top: 165px;
    }

    .rounded-icon-btn {
        top: 261px;
    }

}</style>