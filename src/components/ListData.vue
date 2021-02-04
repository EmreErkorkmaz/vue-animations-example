<template>
    <!-- <ul> -->
      <transition-group tag="ul" name="user-list">
        <li v-for="user in users" :key="user" @click="removeUser(user)">{{ user }}</li>
      </transition-group>
    <!-- </ul> -->
    <div>
      <input v-model="newUser" type="text" />
      <button @click="addUser">Add User</button>
    </div>
</template>

<script>
export default {
    data(){
        return {
            users: ['Emre', 'Seda', 'Ekin', 'Anıl', 'Zeki'],
            newUser: ''
        }
    },
    methods: {
        removeUser(username){
            this.users = this.users.filter(user => user !== username);
        },
        addUser(){
            if(this.newUser.trim().length > 0){
                this.users.unshift(this.newUser);
                this.newUser = '';
            }
        }
    }
}
</script>

<style scoped>
ul {
    list-style: none;
    margin: 1rem 0;
    padding: 0;
}
li {
    border: 1px solid #ccc;
    padding: 1rem 8rem;
    text-align: center;
    cursor: pointer;
}
.user-list-enter-from{
    opacity: 0;
    transform: translateX(-30px);
}
.user-list-enter-active,
.user-list-leave-active{
    transition: all 1s ease-out;
}
.user-list-enter-to,
.user-list-leave-from{
    opacity: 1;
    transform: translateX(0);
}

.user-list-leave-active{
    transition: all 1s ease-in;
    position: absolute;
}
.user-list-leave-to{
    opacity: 0;
    transform: translateX(30px);
}
.user-list-move {
    transition: transform 1s ease;
}
</style>