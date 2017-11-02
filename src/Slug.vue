<template>
    <input id="slug" class="vue-slug" type="text" v-model="mutableValue" @change="onChange">
</template>

<script>
    export default {
        props: {
            /**
             * An reactive property to watch so that we can update the slug component.
             *
             * @type String
             */
            input: {
                type: String,
                default: ''
            },
            /**
             * The currently selected value. This property is passed into the mutableValue data when the component is
             * initialised. We use this to pass the initial value into the component when using v-model.
             *
             * @type String
             */
            value: {
                type: String,
                default: ''
            }
        },
        data() {
            return {
                mutableValue: '',
                shouldSlugInput: true
            };
        },
        methods: {
            /**
             * When the component's content is changes this callback is triggered. This opportunity is taken to update
             * the state of the component and determine whether or not we should slug the input. In addition, we make
             * sure that the current value of the component is converted into a slug.
             */
            onChange() {
                this.updateState();

                this.mutableValue = this.slugify(this.mutableValue);
            },
            /**
             * Update the state of the component. Set the shouldSlugInput data item by determining whether or not the
             * slugged current value of the input matches the current slug.
             */
            updateState() {
                this.shouldSlugInput = this.mutableValue === '' || this.mutableValue === this.slugify(this.input);
            },
            /**
             * Convert the given text into a slug.
             *
             * @param string text
             * @return string
             */
            slugify(text) {
                if (!typeof text === 'string') {
                    return '';
                }

                return text.toString().toLowerCase().trim() // Convert to string, lowercase and trim off whitespace
                    .replace(/&+/gi, 'and')                 // Replace 1 or more & characters with the word 'and'
                    .replace(/\s+/g, '-')                   // Replace spaces with -
                    .replace(/[^\w\-]+/g, '')               // Remove all non-word chars
                    .replace(/--+/g, '-');                  // Replace multiple - with single -
            }
        },
        watch: {
            /**
             * When the watched reactive data property is changed and the component state says we should slug the input,
             * we slug the watched value and update our mutableValue.
             */
            input() {
                if (this.shouldSlugInput) {
                    this.mutableValue = this.slugify(this.input);
                }

                this.updateState();
            },
            /**
             * Whenever the mutable value is changed, we emit an event to parent components indicating that the value of
             * the component has changed. This allows parent components to update their models and forms the basis of
             * two-way binding.
             */
            mutableValue() {
                this.$emit('input', this.mutableValue);
            }
        },
        /**
         * When the component is initally created, the value passed in via v-model is copied into mutableValue and the
         * state of the component is initialised.
         */
        created() {
            this.mutableValue = this.value;
            this.updateState();
        }
    }
</script>

<style>
    .vue-slug {

    }
</style>
