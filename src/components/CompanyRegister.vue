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

                    <div @click="dialog = true"  class="add-btn-input" id="meuBotao">
                        <div class="rounded-icon-input">
                            <img class="list-icon-input" src="@/assets/list.png" alt="Add Icon">
                        </div>
                        <div class="content-items">
                            <p class="item company">Nome: {{ company.name }}</p>
                            <div class="item-flex">
                                <p class="item">CNPJ: {{ company.cnpj }}</p>
                                <p class="item">Email: {{ company.email }}</p>

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
                                <input class="input" type="text" id="cnpj" v-model="cnpj" />
                            </v-col>
                            <v-col cols="12">
                                <label class="label">E-mail</label>
                                <input class="input" type="text" id="email" v-model="email" />
                            </v-col>
                            <v-alert v-if="showAlert" border="left" color="red" type="error">
                                Preenchaos os campos
                            </v-alert>
                        </v-row>
                    </v-form>
                </v-card-text>
                <v-card-actions class="content-btn">

                    <v-btn color="delete-btn " @click="excluirEmpresa(company.id)">
                        <img class="delete-btn-img" src="../assets/delete.png" />
                    </v-btn>

                    <div class="container-btn">
                        <v-btn color="btn-cancel" @click="closeDialog">Cancelar</v-btn>
                        <v-btn color="btn-register" @click="createCompany">Cadastrar</v-btn>
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
            showAlert: false, // Initially, the alert is hidden
            preventRegistration: false, // Initially, registration is not prevented
            // ... other data properties ...
            companies: [],
            filteredCompanies: [],
            headers: [
                { text: "Nome", value: "name" },
                { text: "CNPJ", value: "cnpj" },
                { text: "Email", value: "email" },
            ],
        };
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
            this.showAlert = false; // Hide the alert
        },
        resetForm() {
            this.nome = "";
            this.cnpj = "";
            this.email = "";
            this.$refs.form.resetValidation();
        },
        searchCompanies() {
            axios.get(`https://outros.opea-uat.solutions/prova/front/api/clients?text=${this.searchText}`).then((response) => {
                this.companies = response.data;
                // console.log(response.data);
            });
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


        editarEmpresa(empresa) {
        this.id = empresa.id;
        this.nome = empresa.name;
        this.cnpj = empresa.cnpj;
        this.email = empresa.email;
        this.dialog = true; // Abra o diálogo para edição
    },
        excluirEmpresa(id) {
  const confirmarExclusao = confirm('Tem certeza de que deseja excluir esta empresa?');
  if (confirmarExclusao) {
    axios.delete(`https://outros.opea-uat.solutions/prova/front/api/clients/${id}`)
      .then(() => {
        // Remova a empresa excluída da lista
        this.companies = this.companies.filter(company => company.id !== id);
      })
      .catch(error => {
        console.error('Erro ao excluir a empresa:', error);
      });
  }
}


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
    border: 1.3px solid rgb(217, 217, 217);
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
    padding-right: 10px;
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
    top: 277px;
    left: 39px;
    z-index: 1;
    height: 40px;
    width: 40px;
    border-radius: 50%;
    background-color: #F5F5F5;
    border: 1px solid rgb(123, 123, 123);
    display: flex;
    justify-content: center;
    align-items: center;
}

.rounded-icon-input {
    /* position: absolute;
    top: 338px;
    left: 39px;
    z-index: 1; */
    height: 40px;
    width: 40px;
    border-radius: 50%;
    background-color: #F5F5F5;
    border: 1px solid rgb(123, 123, 123);
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
    border-radius: 3px;
    margin: 0 20px 0 0 !important;
}

.btn-register {
    background-color: #630A37 !important;
    color: white;

}

.divider {
    margin: 0;
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

.search-company {
    text-align: center;
    font-size: 13px !important;
    font-weight: bold !important;
    color: #969696 !important;
    padding-top: 20px;
}
</style>