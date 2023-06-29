<template>
    <div>
        <h2>Customer Installment Page</h2>

        <div v-if="loading"> Loading ...</div>
        <h4 v-if="loaded">List of available customer installments</h4>
      

        <!-- {{ customerInstallments }}-->

        <p v-if="statusCode === 500">The backend service is unavalable now. Please wait</p>



        <p v-if="numberOfCustomers < 0">Installments not found</p>

        <p v-if="numberOfCustomers > 0">Total Number of Customers: <span>{{ numberOfCustomers }}</span></p>

        <table v-if="numberOfCustomers > 0" >
        
            <tr>
                
                <th>Customer Account Name</th>
                <th>Installments Amount in Figure</th>
                <th>Total Number of Installments</th>
                <th style="width: 20%;">Administrative Operations</th>

           
            </tr>
            <tr v-for="installments, customer  in customerInstallments.customer_installments" :key="sl">

             

                <td> {{ customer }}</td>

                <td> <span class="amount" v-for="installment in installments">{{ Math.floor(installment.amount) }}£  </span></td>

                <td>{{ installments.length }}</td>

                <td class="action-button-group">
                    <button class="pay"> PAY</button>
                    <button class="edit">Edit</button>
                    <button class="delete">Delete</button>
                 
                </td>

              
              
            
        
                
              
        
        
            </tr>
        </table>



    </div>
</template>

<script type="module">

import axios from 'axios';
export default{
    data(){
        return {

            customerInstallments: {},
            statusCode:null,
            numberOfCustomers: null,
            loading:true,
            loaded:false
       
        }
    },

 

    mounted(){
        console.log('Mounted');
        console.log("Number of customer installments are: " + this.customerInstallments.length);

        const baseUri = 'http://127.0.0.1:8000/api/customer-installments';

        let token = localStorage.getItem('access_token');

        axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;


        axios.get(baseUri).then((res) => {

            //console.log(res.data);

            this.customerInstallments = res.data;

            //console.log(this.customerInstallments);

            console.log(this.customerInstallments);

            console.log(this.numberOfCustomers);

            this.numberOfCustomers = Object.keys(this.customerInstallments.customer_installments).length;

            console.log(this.numberOfCustomers);

            this.loaded = true;
            this.loading = false;


        }).catch(
            (err) => {
                console.log(err);
                console.log(err.response.status);
                this.statusCode = err.response.status;

            });


    }

    
}

</script>

<style  scoped>
table {
    border: 1px solid skyblue;
    border-radius: 5px;
    padding: 10px 20px;
    border-radius: 5px;
    

    width: 100%;
    background-color: whitesmoke;

}

th {
    background-color: skyblue;
    font-weight: bold;
    border-radius: 5px;
    padding: 5px 0;
    

}

tr {
    padding: 5px 0;
  
}
td{
    
      border: 1px solid skyblue;
      padding: 7px 5px;
}
td:hover{
    background-color: #FFFFFF;
}

.sl {
    font-weight: 900;
}

.amount{
    font-weight: bold;
    background-color: rgb(124, 206, 206);
    margin: 0px 2px;
}
.pay,.edit,.delete{
    padding: 5px 10px;
    margin: 0 2px;
}
.pay{
    background-color: green;
    border: 1px solid green;
    border-radius: 2px;
    color:white;
}

.action-button-group{
    display: flex;
    align-items: center;
    justify-content: space-evenly;
}
</style>