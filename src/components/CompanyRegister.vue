<template>
    <div class="company-register">
        <NavBar />
        <div class="container-input">
            <div class="input-content">
                <input class="form-control" type="search" placeholder="Buscar empresa..." aria-label="Search">
                <img class="img-search" src="@/assets/search.png" alt="Search Icon">
            </div>
            <div class="add-company-input">
                <div class="rounded-icon">
                    <img class="list-icon" src="@/assets/list.png" alt="Add Icon">
                </div>
                <v-btn class="add-btn" @click="dialog = true" id="meuBotao">Adicionar Empresa</v-btn>
            </div>
        </div>

        <v-dialog v-model="dialog" persistent max-width="390">
            <v-card>
                <v-card-title class="content-card">
                    <v-card-title class="title-modal">Cadastrar Empresa</v-card-title>
                    <v-btn class="close-btn">
                        <v-img class="close-modal-register" @click="dialog = false" src="../assets/close.png"></v-img>
                    </v-btn>
                </v-card-title>

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
                        </v-row>
                    </v-form>
                </v-card-text>
                <v-card-actions class="content-btn">

                    <v-btn color="delete-btn " @click="dialog = false">
                        <img class="delete-btn-img" src="../assets/delete.png" />
                    </v-btn>

                    <div class="container-btn">
                        <v-btn color="btn-cancel" @click="dialog = false">Cancelar</v-btn>
                        <v-btn color="btn-register" @click="dialog = false">Cadastrar</v-btn>
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
            dialog: false,
            id: '',
            nome: '',
            email: '',
            cnpj: '',
           
        };
    },
    methods: {

    }
}
</script>


<style scoped >
.company-register {
    background-color: #F5F5F5;
    height: 100vh;
}

.container-input {
    padding: 50px 30px 0 30px;
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
    position: fixed;
}

.add-company-input {
    padding-top: 40px;
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

.v-btn {
    text-transform: none;
}

.rounded-icon {
    position: absolute;
    top: 253px;
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

.list-icon {
    height: 25px;

}

.title-modal {
    color: #969696;
    font-size: 13px !important;
}

.content-card {
    display: flex;
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
.close-btn{
    background-color: white !important;
    box-shadow: none;
    width: 0px;
}
::v-deep .close-btn:hover{
    background-color: rgba(201, 159, 159, 0)!important;
}
</style>