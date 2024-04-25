<template>
    <div class="wrapper">
        <div class="canvas">
            <canvas ref="canvas" width="500" height="500" @mousemove="move" @click="click"></canvas>
        </div>
        <div class="header">
            <ul class="menu">
                <li class="menu-elem menu-button"  v-on:click="showModal_AddFile">
                    <p class="add-image">Добавить изображение</p>
                </li>
            </ul>
        </div>
        <modalAddFile v-show="isModalVisible" @close="closeModal_AddFile"></modalAddFile>
        <div class="info-panel">
            <div class="top-panel">
                <p>Координаты: {{ pixelCoords }}</p>
            <p class="top-panel-text">Изображение ширина : {{ imgWidth }} px</p>
            <p class="top-panel-text">высота : {{ imgHeight }} px</p>
            </div>
            <div class="bottom-panel">
                <p>Выбранный пиксель:</p>
                <p>Цвет: {{ selectedColor }}</p>
                <div class="color" ref="color"></div>
            </div>
        </div>
    </div>
</template>

<script>
// import ModalWindow from '../ModalPage.vue';
import modalAddFile from '../modules/AddFileModal.vue';
export default {
    data() {
        return {
            imgWidth: 0,
            imgHeight: 0,
            pixelColor: "",
            pixelCoords: "",
            selectedColor: "",
            selectedCoords: "",
            isModalVisible: false,
        };
    },
    components: {
        // ModalWindow
        modalAddFile,
    },
    methods: {
        showModal: function () {
                this.$refs.modal.show = true
            },
        showModal_AddFile() {
            this.isModalVisible = true;
            console.log(this.isModalVisible)
        },
        closeModal_AddFile() {
            this.isModalVisible = false;
            console.log(this.isModalVisible)
        },
        move(event) {
            const canvas = this.$refs.canvas;
            const ctx = canvas.getContext("2d");
            const rect = canvas.getBoundingClientRect();
            const x = event.clientX - rect.left;
            const y = event.clientY - rect.top;
            const pxlData = ctx.getImageData(x, y, 1, 1).data;
            this.pixelColor = `rgb(${pxlData[0]}, ${pxlData[1]}, ${pxlData[2]})`;
            this.pixelCoords = `(${Math.round(x)}, ${y})`;
        },
        click(event) {
            const canvas = this.$refs.canvas;
            const ctx = canvas.getContext("2d");
            const rect = canvas.getBoundingClientRect();
            const x = event.clientX - rect.left;
            const y = event.clientY - rect.top;
            const pxlData = ctx.getImageData(x, y, 1, 1).data;
            this.selectedColor = `rgb(${pxlData[0]}, ${pxlData[1]}, ${pxlData[2]})`;
            this.selectedCoords = `(${x}, ${y})`;
            const color = this.$refs.color; 
            color.style.backgroundColor = `rgb(${pxlData[0]}, ${pxlData[1]}, ${pxlData[2]})`;
        },
        fileInput(event) {
            this.file = event.target.files[0];
        },
        navigate() {
            this.$router.push('/');
        },
    },
    mounted() {
        const canvas = this.$refs.canvas;
        const ctx = canvas.getContext("2d");
        const image = new Image();
        image.onload = () => {
            canvas.width = image.width;
            canvas.height = image.height;
            ctx.drawImage(image, 0, 0);
            this.imgWidth = image.width;
            this.imgHeight = image.height;
        };
        const imgData = localStorage.getItem("uploaded_file");
        if (imgData) {
            image.src = imgData;
        } else {
            alert('Изображение не найдено');
            this.$router.push('/');
        }
    },
};
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
}

.header {
    position: fixed;
    height: 60px;
    background-color: #ffffff;
}

.menu-elem, .menu-button {
    background-color: #ffffff;
}

.top-panel-text {

}

.add-image {
    max-width: 200px;
    padding: 5px;
    border: 1px solid rgb(245, 217, 63);
    border-radius: 10px;
    margin-top: 20px;
    cursor: pointer;
}

.wrapper {
    max-width: 100vw;
    width: 100%;
    display: flex;
}

.canvas {
    height: 85vh;
    overflow: scroll;
    background-color: white;
    max-width: 100vw;
    padding-top: 30px;
    margin: 50px auto 0;
}

form {
    max-width: 150px;
    width: 100%;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 10px;
}

input {
    width: 150px;
    height: 30px;
    margin-bottom: 10px;
}
button {
    cursor: pointer;
    margin-top: 50px;
    width: 150px;
    height: 50px;
    border-width: 3px;
    border-color: rgb(73, 152, 255);
    background-color: rgb(73, 152, 255);
}
button:hover {
    transition: .3s;
    background-color: white;
}

.info-panel {
    position: fixed;
    bottom: 0;
    left: 0;
    max-width: 100vw;
    width: 100%;
    height: 20px;
    background-color: #3a3a3a;
    color: #000000;
    padding: 10px;
    display: flex;
}
.top-panel {
    display: flex;
    padding-left: 30px;
    /* justify-content: center; */
    align-items: center;
    background-color: #3a3a3a;
}

.top-panel p {
    
    background-color: #3a3a3a;
}

.bottom-panel {
    display: flex;
    /* flex-direction: column; */
    justify-content: center;
    align-items: center;
    background-color: #3a3a3a;
}

.bottom-panel p {
    background-color: #3a3a3a;
}

.color {
    width: 20px;
    height: 20px;
    border: 3px solid white;
    margin-left: 10px;
}
p {
    margin-left: 10px;
}
</style>