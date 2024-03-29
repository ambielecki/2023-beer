<template>
    <div>
        <div class="columns is-multiline">
            <div class="column is-half">
                <div class="field">
                    <label class="label" for="name">Name</label>
                    <div class="control">
                        <input
                            id="name"
                            class="input"
                            type="text"
                            placeholder="Name your beer"
                            v-model="$v.beer.name.$model"
                            data-test="form-name"
                        >
                    </div>
                    <p class="help is-danger" v-if="$v.beer.name.$anyError">
                        A Name is Required
                    </p>
                </div>
            </div>

            <div class="column is-half">
                <div class="field">
                    <label class="label" for="style">Style</label>
                    <div class="control is-expanded">
                        <div class="select is-fullwidth">
                            <select
                                id="style"
                                v-model="beer.style"
                                data-test="form-style"
                            >
                                <option :value="null" disabled>Select Style</option>
                                <option
                                    v-for="(name, value) in styles"
                                    :value="value"
                                    :key="value"
                                >
                                    {{ name }}
                                </option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>

            <div class="column is-half">
                <div class="field">
                    <label class="label" for="yeast">Yeast Strain</label>
                    <div class="control">
                        <input
                            id="yeast"
                            class="input"
                            type="text"
                            placeholder="Yeast Strain"
                            v-model="beer.yeast"
                            data-test="form-yeast"
                        >
                    </div>
                </div>
            </div>

            <div class="column is-half">
                <div class="field">
                    <label class="label" for="primary_fermentation_start">Primary Fermentation Start</label>
                    <datepicker
                        placeholder="Select Date"
                        id="primary_fermentation_start"
                        v-model="$v.beer.primary_fermentation_start.$model"
                        :config="{
                            dateFormat: 'Y-m-d H:i',
                            enableTime: true,
                            static: true
                        }"
                    ></datepicker>
                    <p class="help is-danger" v-if="$v.beer.primary_fermentation_start.$anyError">
                        Start Date is Required
                    </p>
                </div>
            </div>

            <div class="column is-half">
                <div class="field">
                    <label class="label" for="secondary_fermentation_start">Secondary Fermentation Start</label>
                    <datepicker
                        placeholder="Select Date"
                        id="secondary_fermentation_start"
                        v-model="beer.secondary_fermentation_start"
                        :config="{
                            dateFormat: 'Y-m-d H:i',
                            enableTime: true,
                            static: true
                        }"
                        data-test="form-secondary"
                    ></datepicker>
                </div>
            </div>

            <div class="column is-half">
                <div class="field">
                    <label class="label" for="bottling">Bottling</label>
                    <datepicker
                        placeholder="Select Date"
                        id="bottling"
                        v-model="beer.bottling"
                        :config="{
                            dateFormat: 'Y-m-d H:i',
                            enableTime: true,
                            static: true
                        }"
                        data-test="form-bottling"
                    ></datepicker>
                </div>
            </div>

            <div class="column is-half">
                <div class="field">
                    <label class="label" for="rating">Rating</label>
                    <div class="control is-expanded">
                        <div class="select is-fullwidth">
                            <select
                                id="rating"
                                v-model="beer.rating"
                                data-test="form-rating"
                            >
                                <option :value="null">Unrated</option>
                                <option
                                    v-for="n in 5"
                                    :value="n"
                                    :key="n"
                                >
                                    {{ n }} {{ n === 1 ? 'Star' : 'Stars' }}
                                </option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="columns is-multiline">
            <div class="column is-half">
                <div class="field" data-test="form-recipe">
                    <label class="label" for="recipe">Recipe</label>
                    <ckeditor
                        :editor="editor"
                        class="textarea"
                        id="recipe"
                        v-model="beer.recipe"
                        placeholder="Recipe"
                    ></ckeditor>
                </div>
            </div>

            <div class="column is-half">
                <div class="field" data-test="form-brew">
                    <label class="label" for="recipe">Brew Notes</label>
                    <ckeditor
                        :editor="editor"
                        class="textarea"
                        id="brew_notes"
                        v-model="beer.brew_notes"
                        placeholder="Brewing Notes"
                        data-test="form-brew"
                    ></ckeditor>
                </div>
            </div>

            <div class="column is-half">
                <div class="field" data-test="form-tasting">
                    <label class="label" for="tasting_notes">Tasting Notes</label>
                    <ckeditor
                        :editor="editor"
                        class="textarea"
                        id="tasting_notes"
                        v-model="beer.tasting_notes"
                        placeholder="Tasting Notes"
                        data-test="form-tasting"
                    ></ckeditor>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
/* eslint-disable */
    import Datepicker from 'vue-bulma-datepicker';
    import CKEditor from '@ckeditor/ckeditor5-vue';
    import ClassicEditor from '@ckeditor/ckeditor5-build-classic';
    import { required } from 'vuelidate/lib/validators';

    export default {
        components: {
            Datepicker: Datepicker,
            ckeditor: CKEditor.component,
        },
        props: ['beer'],
        validations: {
            beer: {
                name: { required },
                primary_fermentation_start: { required },
            },
        },
        computed: {
            styles: function () {
                return this.$store.state.styles;
            }
        },
        data: function () {
            return {
                editor: ClassicEditor,
            };
        },
        methods: {
            validate: function () {
                this.$v.$touch();

                if (this.$v.$anyError) {
                    this.$store.commit('addMessage', {
                        time: 5,
                        type: 'is-danger',
                        message: 'Please fix form errors before submission',
                    });

                    return false;
                }

                return true;
            },
        },
    };
</script>

<style>
    .ck-editor__editable {
        min-height: 200px;
    }
</style>