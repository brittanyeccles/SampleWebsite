<template>
    <NavigationBar current="Contact Us" />
    <div class="alert alert-success alert-dismissible" style="display: none" id="submit-success-msg">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        Your form has successfully been submitted. You should hear back from us in 1-2 business days.
    </div>
    <div class="alert alert-danger alert-dismissible" style="display: none" id="submit-error-msg">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        An error has occured while submitting your form. Please try again later.
    </div>
    <form>
        <h5 id="header-text">Please fill out the form below to contact us.</h5>
        <div class="form-group">
            <p class="error-msg" id="firstName-err">Please input a valid first name.</p>
            <label for="firstName" class="sr-only">First Name</label>
            <input type="text" class="form-control" id="firstName" placeholder="First Name" v-model="form.firstName" />
        </div>
        <div class="form-group">
            <p class="error-msg" id="lastName-err">Please input a valid last name.</p>
            <label for="lastName" class="sr-only">Last Name</label>
            <input type="text" class="form-control" id="lastName" placeholder="Last Name" v-model="form.lastName" />
        </div>
        <div class="form-group">
            <p class="error-msg" id="email-err">Please input a valid email address.</p>
            <label for="email" class="sr-only">Email</label>
            <input type="email" class="form-control" id="email" placeholder="Email" v-model="form.email" />
        </div>
        <div class="form-group">
            <label for="questionType">I would like to ask a question regarding:</label>
            <select class="form-control" id="questionType" v-model="form.questionType">
                <option value="order">Previous Order</option>
                <option value="item">Item Stock</option>
                <option value="other">Other</option>
            </select>
        </div>
        <div v-if="form.questionType == 'order'" class="form-group">
            <p>If you have an order number associated with your question, please provide it below.</p>
            <div class="form-group">
                <label for="orderNumber" class="sr-only">Order Number</label>
                <input type="text" class="form-control" id="orderNumber" placeholder="Order Number" v-model="form.orderNumber" />
            </div>
        </div>
        <div v-if="form.questionType == 'item'">
            <p>If you have an item number associated with your question, please provide it below.</p>
            <div class="form-group">
                <label for="itemNumber" class="sr-only">Item Number</label>
                <input type="text" class="form-control" id="itemNumber" placeholder="Item Number" />
            </div>
        </div>
        <div class="form-group">
            <p class="error-msg" id="questionText-err">Please input your question below.</p>
            <label for="questionText" class="sr-only">Please input your question</label>
            <textarea class="form-control" id="questionText" rows="4"
                placeholder="Please input your question" v-model="form.questionText"></textarea>
        </div>
        <button id="submit-btn" type="submit" class="btn btn-primary" @click="submitForm()">Submit</button>
    </form>
</template>

<script>
import NavigationBar from './components/NavigationBar.vue';

global.jQuery = require('jquery');
var $ = global.jQuery;
window.$ = $;

export default {
    name: 'App',
    components: {
        NavigationBar
    },
    data: function() {
        return {
            form: {
                firstName: "",
                lastName: "",
                email: "",
                questionType: "order",
                orderNumber: "",
                itemNumber: "",
                questionText: "",
            }
        }
    },

    watch: {
        // As the question type changes, reset the values for the other question types
        "questionType": function(value) {
            if (value) {
                if (value == "order") {
                    this.form.itemNumber = "";
                    this.form.questionText = "";
                } else if (value == "item") {
                    this.form.orderNumber = "";
                    this.form.questionText = "";
                } else {
                    this.form.questionText = "";
                }
            }
        }
    },

    methods: {
        // Validate each of the required fields before saving the information submitted
        submitForm: function() {
            // Reset error messages and styles before beginning to validate
            this.resetErrorStyles("firstName", "lastName", "email", "questionText");

            // Capture invalid inputted items' IDs
            let invalidItems = [];
            if (this.form.firstName == null || this.form.firstName == '') {
                invalidItems.push("firstName");
            }
            if (this.form.lastName == null || this.form.lastName == '') {
                invalidItems.push("lastName");
            }
            if (this.form.email == null || this.form.email == '') {
                invalidItems.push("email");
            }
            if (this.form.questionText == null || this.form.questionText == "")  {
                invalidItems.push("questionText");
            }

            // Send invalid elements to the setErrorStyles method to apply error styles
            if (invalidItems.length > 0) {
                this.setErrorStyles(invalidItems);
            } else {
                // If there are no errors, then reset the error messages and styles and 
                // send validated information to the backend
                this.resetErrorStyles("firstName", "lastName", "email", "questionText");

                $("#submit-success-msg").fadeIn();

                /*
                    let vm = this;                    
                    fetch("/contactUs/submit", {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json",
                        },
                        body: JSON.stringify({
                            form: vm.form
                        })
                    })
                    .then (response => {
                        if (!response.ok) {
                            $("#submit-error-msg").fadeIn();
                        } else {
                            $("#submit-success-msg").fadeIn();
                        }
                    })
                */
            }
        },

        // Used to reset all of the error styles for the error messages and the selections
        // that were previously invalid
        resetErrorStyles: function(...elementIds) {
            for (let i = 0; i < elementIds.length; i++) {
                document.getElementById(elementIds[i]).classList.remove("error-input-field");
                document.getElementById(elementIds[i] + "-err").style.display = "none";
            }
        },

        // Used to display the proper error message string on the invalid inputted field, as well as
        // adding the correct styles to the input box and setting the focus on the input box
        setErrorStyles: function(...elementIds) {
            let elements = elementIds[0];
            for (let i = 0; i < elements.length; i++) {
                document.getElementById(elements[i]).className += " error-input-field";
                document.getElementById(elements[i] + "-err").style.display = "block";
            }
        }
    }
}
</script>

<style>
body {
    background-color: #f8f9fa;
}
form {
    width: 40%;
    margin: 4em auto;
    padding: 15px;
}
.error-msg {
    color: red;
    margin-bottom: 7px;
    display: none;
    text-align: left;
}
.error-input-field {
    border: 2px solid red;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#welcome-img {
    width: 100%;
}
#submit-btn {
    width: 50%;
}
#header-text {
    margin: 7px 0 15px 0;
}

#submit-success-msg, #submit-error-msg {
    margin-top: 75px;
}

@media screen and (max-width: 1150px) {
    form {
        width: 75%;
    }
}
@media screen and (max-width: 675px) {
    form {
        width: 100%;
    }
}

</style>
