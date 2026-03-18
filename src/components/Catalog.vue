<script>

export default {
    data() {
        return {
            films: [],
            customers: []
        }
    },
    methods: {
        getFilms() {
            this.films = [];
            fetch('http://127.0.0.1:8000/api/films')
                .then(response => response.json())
                .then(data => {
                    data.forEach(d => {
                        this.films.push(d);
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
        }
    },
    mounted() {
        this.getCustomers()
        this.getFilms()
    }
}
</script>

<template>
    <div class="row">

        <div class="col-md-6">
            <strong>Customers</strong>
            <div class="list-group">
                <div class="list-group-item" v-for="customer in customers">
                    {{ customer.name }}
                </div>

            </div>
        </div>

        <div class="col-md-6">
            <strong>Films</strong>
            <div class="list-group">
                <div class="list-group-item" v-for="film in films">
                    {{ film.title }}
                </div>
            </div>
        </div>


    </div>
</template>