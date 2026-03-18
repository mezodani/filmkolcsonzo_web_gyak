<script>
export default {
    data() {
        return {
            films: [],
            customers: [],
            newRental: {
                film_id: '',
                customer_id: '',
                due_date: ''
            },
            rentals: []
        }
    },
    methods: {
        getFilms() {
            this.films = [];
            fetch('http://127.0.0.1:8000/api/films')
                .then(response => response.json())
                .then(data => {
                    data.forEach(d => {
                        if (d.available == 1) {
                            this.films.push(d);
                        }
                    });
                })
        },
        getCustomers() {
            this.customers = []
            fetch('http://127.0.0.1:8000/api/customers')
                .then(response => response.json())
                .then(data => {
                    data.forEach(d => {
                        this.customers.push(d);
                    });
                })
        },
        postRental() {
            fetch('http://127.0.0.1:8000/api/rentals', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Accept': 'application/json'
                },
                body: JSON.stringify(this.newRental)
            })
                .then(response => response.json())
                .then(data => {
                    alert(data.message)
                    this.getCustomers()
                    this.getFilms()
                    this.getRentals()
                })
        },
        getRentals() {
            this.rentals = [];
            fetch('http://127.0.0.1:8000/api/rentals')
                .then(response => response.json())
                .then(data => {
                    data.forEach(d => {
                        this.rentals.push(d)
                    });
                })
        },
        deleteRental(id) {
            fetch(`http://127.0.0.1:8000/api/rentals/${id}`, {
                method: "DELETE",
                headers: {
                    'Content-Type': 'application/json',
                    'Accept': 'application/json'
                }
            })
            .then(response => response.json())
            .then(data => {
                alert(data.message)
                this.getCustomers()
                this.getFilms()
                this.getRentals()
            })
        }
    },
    mounted() {
        this.getFilms()
        this.getCustomers()
        this.getRentals()
    }
}
</script>

<template>
    <div class="row">
        <div class="col-md-6">
            <form>
                <div class="mb-3">
                    <label for="film" class="form-label">Film</label>
                    <select id="film" class="form-select" size="10" v-model="newRental.film_id">
                        <option v-for="film in films" :key="film.id" :value="film.id">{{ film.title }}</option>
                    </select>
                    <label for="customer" class="form-label">Customer</label>
                    <select id="customer" class="form-select" size="10" v-model="newRental.customer_id">
                        <option v-for="customer in customers" :key="customer.id" :value="customer.id"> {{ customer.name
                            }}</option>
                    </select>
                    <label for="due_date">Due date</label>
                    <input type="date" class="form-control" v-model="newRental.due_date">
                </div>
                <input type="button" class="btn btn-primary" value="Submit" @click="postRental">
            </form>
        </div>
        <div class="col-md-6">
            <div class="list-group">
                <div class="list-group-item" v-for="rental in rentals">
                    customer: {{ rental.customer.name }}
                    film: {{ rental.film.title }}
                    <input type="button" class="btn btn-danger float-end" value="x" @click="deleteRental(rental.id)">
                </div>
            </div>
        </div>
    </div>
</template>