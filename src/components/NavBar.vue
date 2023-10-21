<template>
    <nav style=" background-color: white!important;" class="navbar bg-body-tertiary">
        <div class="navbar-content">
            <a class="navbar-brand" href="#">
                <img src="../assets/logo.png" alt="Bootstrap" width="70" height="24">
            </a>
            <div class="content-user">
                <p class="user">Nome do Usuário</p>
                <v-img class="user-icon" src="../assets/user.png"></v-img>
            </div>


        </div>
    </nav>
</template>
<style>

.navbar-content {
    padding: 0 20px 0 20px;
    display: flex;
    justify-content: space-between;

}

nav.navbar.bg-body-tertiary {
    display: inherit;
}

.content-user {
    display: flex;
}

.user {
    color: #969696;
    padding-top: 8px;
}

.user-icon {
    margin: 0 20px 0 20px;
    height: 30px;
    width: 30px;
}

.v-card.v-sheet.theme--light {
    padding: 0px;
}

.img {
    height: 10px;
    width: 10px;
}</style>