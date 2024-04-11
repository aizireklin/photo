<template>
    <div class="modal-backdrop">
        <div class="modal">
        <section class="modal-body">
            <slot name="body">
                <label for="imageInput">Добавить файл:</label>
                <input type="file" id="imageInput" ref="fileInput" @change="handleFileInputChange">
                <label for="urlInput">укажите URL:</label>
                <input type="text" id="urlInput" v-model="imageUrl">
            </slot>
        </section>
        <footer class="modal-footer">
            <slot name="footer">
                <button class="btn-green" @click="handleSubmit">Отправить</button>
                <button type="button" class="btn-green" @click="close()">Закрыть</button>
            </slot>
        </footer>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'modalAddFile',
        data() {
            return {
                imageUrl: '',
                file: null
            };
        },
        methods: {
            close() {
                // location.reload();
                this.$emit('close');
            },
            handleFileInputChange(event) {
                this.file = event.target.files[0];
            },
            handleSubmit() {
                if (this.imageUrl.trim() !== '') {
                    localStorage.setItem("uploaded_file", this.imageUrl);
                    this.navigateToRedactorPage();
                } else if (this.file !== null) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                    localStorage.setItem("uploaded_file", e.target.result);
                        this.navigateToRedactorPage();
                    };
                    reader.readAsDataURL(this.file);
                } else {
                    alert('Пожалуйста, выберите изображение или укажите URL');
                }
            },
            navigateToRedactorPage() {
                location.reload();
            }
        },
    };
</script>

<style>
.modal-backdrop {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background-color: rgba(0, 0, 0, 0.3);
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .modal {
        background: #FFFFFF;
        box-shadow: 2px 2px 20px 1px;
        overflow-x: auto;
        display: flex;
        flex-direction: column;
        max-width: 350px;
    }

    .modal-header,
    .modal-footer {
        padding: 15px;
        display: flex;
    }

    .modal-header {
        border-bottom: 1px solid #eeeeee;
        color: #4AAE9B;
        justify-content: space-between;
    }

    .modal-footer {
        border-top: 1px solid #eeeeee;
        justify-content: center;
        gap: 10px;
    }

    .modal-body {
        position: relative;
        padding: 20px 10px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 20px;
    }

    .btn-close {
        border: none;
        font-size: 20px;
        padding: 20px;
        cursor: pointer;
        font-weight: bold;
        color: #dadd3d;
        background: transparent;
    }

    .btn-green {
        color: white;
        background-color: #d3c335;
        color: #141414;
        border: 1px solid #d3c335;
        border-radius: 2px;
        padding: 10px;
        width: 100px;
        cursor: pointer;
    }
</style>