<template>
    <v-row justify="center">
        <v-dialog v-model="dialog" persistent max-width="600px">

            <template v-slot:activator="{ on, attrs }">
                <v-btn color="primary" dark v-bind="attrs" v-on="on">
                    Open Dialog
                </v-btn>

            </template>
            <v-card>
                <v-card-title>
                    <span class="text-h5">Cadastrar empresa</span>
                    <v-spacer></v-spacer>
                    <v-btn text @click="dialog = false">
                        <v-img src="../assets/close.png"></v-img>
                    </v-btn>

                </v-card-title>
                <v-divider></v-divider>
                <v-card-text>
                    <v-container>

                        <v-row>
                            <v-col cols="12">
                                <v-text-field label="Nome" required></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="CNPJ" type="password" required></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field label="E-mail" required></v-text-field>
                            </v-col>

                        </v-row>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false">
                        <v-img class="icon-delete" src="../assets/delete.png"></v-img>
                    </v-btn>

                    <v-btn color="blue darken-1" text @click="dialog = false">
                        Cancelar </v-btn>
                    <v-btn color="blue darken-1" text @click="dialog = false">
                        Cadastrar
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-row>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            dialog: false
        }
    }
};
</script>
<style>
.icon-delete {
    width: 1px;
}

.v-dialog {
    overflow: hidden;

}

.v-card.v-sheet.theme--light {
    padding: 20px;
}

.v-btn:before {
    background-color: rgba(255, 255, 255, 0);
}

.v-btn:hover {
    background-color: rgba(255, 255, 255, 0);
}

.v-input__slot {
    border: 1px solid rgb(137, 137, 137);
    border-radius: 4px !important;
    height: 50px;
}

.v-text-field input {
    padding: 8px;
}


</style>