<template>
    <div>
        <NavBar />

        <v-container>
            <v-row>
                <v-col cols="12">
                    <v-text-field
                        v-model="searchText"
                        label="Pesquisar Empresas"
                        append-icon="mdi-magnify"
                        @click:append="searchCompanies"
                    ></v-text-field>
                </v-col>
            </v-row>

            <v-row>
                <v-col cols="12">
                    <v-data-table :headers="headers" :items="companies" :search="searchText"></v-data-table>
                </v-col>
            </v-row>

            <v-row>
                <v-col cols="12">
                    <v-btn color="btn-register" @click="openDialog">Cadastrar Empresa</v-btn>
                </v-col>
            </v-row>
        </v-container>

        <v-dialog v-model="dialog" max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="text-h5">Cadastrar Empresa</span>
                </v-card-title>
                <v-card-text>
                    <v-form ref="form" v-model="valid" lazy-validation>
                        <v-row>
                            <v-col cols="12">
                                <label class="label">Nome</label>
                                <input class="input" type="text" id="name" v-model="nome" />
                            </v-col>
                            <v-col cols="12">
                                <label class="label label-cnpj">CNPJ</label>
                                <input class="input input-cnpj" type="text" id="cnpj" v-model="cnpj" />
                            </v-col>
                            <v-col cols="12">
                                <label class="label">Email</label>
                                <input class="input" type="text" id="email" v-model="email" />
                            </v-col>
                        </v-row>
                    </v-form>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="closeDialog">Cancelar</v-btn>
                    <v-btn color="blue darken-1" text @click="createCompany">Cadastrar</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>

<script>
import NavBar from "./NavBar";
import axios from "axios";
export default {
    components: {
        NavBar,
    },
    data() {
        return {
            searchText: "",
            dialog: false,
            valid: true,
            nome: "",
            cnpj: "",
            email: "",
            companies: [],
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
            });
        },
        createCompany() {
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
        },
    },
};
</script> 