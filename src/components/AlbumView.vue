<template>
<transition name="modal" v-if="album && album._id == albumId">
    <div class="modal-mask">
        <div class="modal-wrapper" @click.self="closeButtonClicked()">
            <div class="modal-container">

                <div class="modal-header row">
                    <slot name="header">
                        <div class="col-md-7 col-sm-12">
                            <h2> {{ album.album_title }} </h2>
                        </div>
                        <div class="col-md-2 col-sm-6">
                            <p> <span class="badge badge-primary">By</span> <a class="colored_name10" :href="'/profile/' + album.user.user_id">{{ album.user.user_name }}</a> </p>
                        </div>
                        <div class="col-md-3 col-sm-6">
                            <p> <span class="badge badge-primary">At</span> <a href="#stay_here" data-toggle="tooltip" title="" data-original-title="">{{ album.updated_at }}</a> </p>
                        </div>
                    </slot>
                </div>

                <div class="modal-body">
                    <slot name="body">
                        <div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
                            <ol class="carousel-indicators">
                                <li data-target="#carouselExampleIndicators" :data-slide-to="index" :class="index==0?'active':''" v-for="(photo, index) in album.photos" v-bind:key="photo._id">></li>
                            </ol>
                            <div class="carousel-inner"  style="background-color:black">
                                <div class="carousel-item aspect" :class="index==0?'active':''" v-for="(photo, index) in album.photos" v-bind:key="photo._id">
                                    <img class="img-respective mx-auto d-block" :src="getPhotoUrl(photo._id)" :alt="photo.photo_name">
                                </div>    
                            </div>
                            <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
                                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                                <span class="sr-only">Previous</span>
                            </a>
                            <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
                                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                                <span class="sr-only">Next</span>
                            </a>
                        </div>                        
                    </slot>
                </div>

                <div class="modal-footer">
                    <slot name="footer">
                        <button class="btn btn-danger modal-default-button" @click="closeButtonClicked()">
                            닫기
                        </button>
                    </slot>
                </div>
            </div>
        </div>
    </div>
</transition>
</template>

<script>
import RestAPI from '../constants/RestAPI';

export default {
    name: 'AlbumView',
    props: {
        albumId: String
    },
    mounted() {
        this.$store.dispatch('fetchAlbum', this.albumId);
    },
    methods: {
        closeButtonClicked() {
            this.$emit('closeModal');
        },
        getPhotoUrl(photoId) {
            return RestAPI.GET_PHOTO + photoId;
        }
    },
    computed: {
        album() {
            return this.$store.getters.getAlbum;
        }
    }
};
</script>

<style>
.aspect {
    height: 0;
    padding-top: 33.25%;
}

.img-respective {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    height: 100%;
    max-height: 100%;
}

.modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .9);
    display: table;
    transition: opacity .3s ease;
}

.modal-wrapper {
    display: table-cell;
    vertical-align: middle;
}

.modal-container {
    width: 80%;
    margin: 0px auto;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
    transition: all .3s ease;
    font-family: Helvetica, Arial, sans-serif;
    max-height: 90%;
}

.modal-header h3 {
    margin-top: 0;
    color: #42b983;
}

.modal-body {
    margin: 20px 0;
}

.modal-default-button {
    float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
    opacity: 0;
}

.modal-leave-active {
    opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
    -webkit-transform: scale(1.1);
    transform: scale(1.1);
}
</style>
