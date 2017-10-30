<template>
    <div>
        <label for="slug">Slug</label>
        <input id="slug" type="text" v-model="slug" @change="onChange">
    </div>
</template>

<script>
    export default {
        props: {
            input: String,
            value: String
        },
        data() {
            return {
                slug: '',
                shouldSlug: true
            };
        },
        methods: {
            onChange() {
                this.checkState();

                this.slug = this.slugify(this.slug);
            },
            checkState() {
                this.shouldSlug = this.slug === '' || this.slug === this.slugify(this.input);
            },
            slugify(text) {
                if (!typeof text === 'string') {
                    return '';
                }

                return text.toLowerCase().trim().replace(/\s+/gi, '-');
            }
        },
        watch: {
            input() {
                if (this.shouldSlug) {
                    this.slug = this.slugify(this.input);
                }

                this.checkState();
            }
        },
        mounted() {
            this.slug = this.value;
            console.log('mounted');
        }
    }
</script>
