<template>
    <div class="company-register">
        <NavBar />
        <div class="container-input">
            <div class="input-content">
                <input class="form-control " type="search" placeholder="Buscar empresa..." aria-label="Search">
                <img class="img-search" src="../assets/search.png" alt="Search Icon">
            </div>
            <div class="add-company-input">
                <div class="rounded-icon">
                    <img class="list-icon " src="../assets/list.png" alt="Add Icon">
                </div>
                <v-btn class="add-btn" type="button" id="meuBotao">Adicionar Empresa </v-btn>
            </div>
        </div>
    </div>
</template>
<script>
import NavBar from './NavBar.vue';
export default {

    name: 'CompanyRegister',
    components: {
        NavBar
    }
}
</script>

<style>
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
    top: 172px;
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
    top: 246px;
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
</style>