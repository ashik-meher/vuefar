<template>
    <div>

 

        <div class="row">
            <div class="col-md-4">
                <p>Here is your profile</p>

                <p>{{ profile.name }}</p>
                <p>{{ profile.email }}</p>

                <button  v-on:click="changeProfileImage" class="change-profile-image-button"><i class="fa fa-file-image-o"></i>Change Profile Image</button>

            </div>

            <div class="col-md-4" >

                <div id="image-display">
                    <!-- Static Image for test-->

                    <!-- <img class="avatar" src="./../assets/logo.svg" alt="">-->

                    <div class="avatar-holder">
                        <img class="avatar" :src="profile.avatarLink"  alt="">

                    </div>

                    
                    <button class="image-update-confirm" v-if="imagePreviewed"><i class="fa fa-check-circle" aria-hidden="true"></i>Confirm</button>


                </div>

              
            </div>

        </div>


        <div id="image-preview" class="row">

            <input ref="imageInputForPreview" id="imageInputForPreview" type="file" accept="image/*" v-on:change="previewImageOnScreen">

            <div></div>

            <img v-if="previewImage" style="width:80px;height: 80px;" :src="previewImage" alt="" >

        </div>

   



    </div>
</template>

<script type="module">
import axios from 'axios';


export default {
    data(){
        return {
            reqUri: 'http://127.0.0.1:8000/api/details',
            profile: {},
            previewImage: null,
            imagePreviewed: false

        }
    },

    mounted(){

        let token = localStorage.getItem('access_token');

        axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

        console.log(axios.defaults.headers.common['Authorization']);

        axios.post(this.reqUri).then((res) => {
            console.log(res.data);

            //write in component data

            this.profile.name = res.data.success.name;
            this.profile.email = res.data.success.email;

            this.profile.avatarLink =   res.data.avatar;


            // Test for middleware

            let user = localStorage.getItem('user');

            console.log(user);

            
        }).catch((err) => console.log(err));

    },

    methods: {
        changeProfileImage(e){
            console.log("Profile Pic Change Request!");

            let token = localStorage.getItem('access_token');

            axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

            //this.previewImage =  this.$refs.imageInputForPreview;

            //var reader = new FileReader(); 

            this.$refs.imageInputForPreview.click();
            



        },

        previewImageOnScreen(e){
            console.log(e.target.files[0]);

            // Accurate

            //this.previewImage = URL.createObjectURL(e.target.files[0]) ;

            this.profile.avatarLink = URL.createObjectURL(e.target.files[0]);
            this.imagePreviewed = true;
        }
    }
}
//https://codepen.io/chenuin/pen/MdvyjG
</script>

<style scoped>

.avatar{
    border-radius: 5%;
    max-width: 90%;
    padding: 5px;
    margin: 5px;
    max-height: 250px;
}

.change-profile-image-button{
  padding: 5px 10px;
  background-color: skyblue;
  border: 5px solid skyblue;
  border-radius: 5px;
}
.image-update-confirm{

 display: block; 
 padding: 10px 10px;
  background-color: skyblue;
  border: 5px solid skyblue;
  border-radius: 5px;

  margin-left: 10px;
  margin-top: 3px;
}

#image-display{
    border: 1px solid whitesmoke;
    align-items: center;
    padding: 10px;
}
#imageInputForPreview{
  padding: 5px 10px;
  background-color: skyblue;
  border: 5px solid skyblue;
  border-radius: 5px;
  display: none;

}
.avatar-holder{
    width: 100%;
    height: 300px;
}
</style>