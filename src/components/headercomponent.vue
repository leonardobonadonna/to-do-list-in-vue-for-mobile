<template>
    <header>
        <div v-if="!selectedList">
            <div class="linea">
                <div class="title">
                    <h1>{{ titolo }}</h1>
                </div>
                <div>
                    <button @click="popup()" class="blu-button">+</button>
                </div>
            </div>

            <transition name="modal">
                <div class="modal" v-if="showModale">
                    <div class="modal-content">
                        <p class="TextPopUp">Inserisci il nome della lista</p>
                        <input type="text" v-model="nomeLista" required id="testo" />
                        <input type="button" value="Inserisci lista" class="SubmitButton" @click="inserisciLista()" :disabled="!nomeLista">
                        <input type="button" value="X" class="CloseButton" @click="chiudipopup()">
                    </div>
                </div>
            </transition>

            <div v-if="liste.length" v-for="(lista, i) in liste" style="display: flex; flex-direction: column;">
                <div style="display: flex; gap: 2rem" class="ListName">
                    <p @click="selectedList = lista"> {{ lista.nomeLista }} <span class="ntask">({{lista.contenutoLista.length > 1 ? 'ci sono ' + lista.contenutoLista.length + ' elementi' : 'c\'è ' + lista.contenutoLista.length  + ' elemento'}} nella lista) </span></p>
                    <button @click="rimuoviLista(i)" class="redButton"> x </button>
                </div>
            </div>
        </div>

        <div v-else>
            <transition name="modal">
                <div class="modal" v-if="showModale2">
                    <div class="modal-content">
                        <p class="TextPopUp">Inserisci il nome della task</p>
                        <input type="text" v-model="nomeTask" required id="testo" />
                        <input type="button" value="Inserisci task" class="SubmitButton" @click="inserisciTask()" :disabled="!nomeTask">
                        <input type="button" value="X" class="CloseButton" @click="ChiudiPopupTask()">
                    </div>
                </div>
            </transition>
            <div class="allineamento">
                <h1>{{ selectedList.nomeLista }}</h1>
                <button @click="PopupTask()" class="blu-button2">+</button>
            </div>
            <div class="listaTask">
                <li v-for="(task, index) in selectedList.contenutoLista" :key="index">
                    {{ task.nomeTask }}
                    <button @click="rimuoviTask(index)" class="redButton">x</button>
                </li>
                <button @click="selectedList = null" class="backButton">Torna all' elenco delle liste</button>
            </div>
        </div>
    </header>
</template>



<script>
export default {
    data() {
        return {
            titolo: "Benvenuto, crea la tua lista",
            nomeLista: '',
            nomeTask: '',
            showModale: false,
            showModale2: false,
            liste: [],
            selectedList: null,
            NumTask: 0,
        };
    },
    methods: {
        popup() {
            this.nomeLista = '';
            this.showModale = true;
        },
        inserisciLista() {
            this.liste.push({ nomeLista: this.nomeLista, contenutoLista: [] });
            this.showModale = false;
            this.nomeLista = '';
            localStorage.setItem('liste', JSON.stringify(this.liste));
        },
        rimuoviLista(index) {
            this.liste.splice(index, 1);
            localStorage.setItem('liste', JSON.stringify(this.liste));
        },
        chiudipopup() {
            this.showModale = false;
        },
        PopupTask() {
            this.nomeTask = '';
            this.showModale2 = true;
        },
        ChiudiPopupTask() {
            this.showModale2 = false;
        },
        inserisciTask() {
            if (this.selectedList) {
                this.selectedList.contenutoLista.push({ nomeTask: this.nomeTask });
                this.showModale2 = false;
                this.nomeTask = '';
                localStorage.setItem('liste', JSON.stringify(this.liste));
              
            }
        },
        rimuoviTask(index) {
            this.selectedList.contenutoLista.splice(index, 1);
            
            localStorage.setItem('liste', JSON.stringify(this.liste));
        }
    },
    mounted() {
        if (localStorage.getItem('liste') != null) {
            this.liste = JSON.parse(localStorage.getItem('liste'));
        }
    }
};
</script>

<style scoped>

.ntask{
    font-family: Montserrat;
    font-weight: bold; 
}
.allineamento {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    text-align: center;
}

.listaTask {
    list-style-type: none;
    padding: 0;
    margin: 20px 0;
}

.listaTask li {
    background-color: #f0f0f0;
    margin: 10px 0;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.listaTask li:hover {
    background-color: #e0e0e0;
}

.modal {
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);
    display: flex;
    align-items: center;
    justify-content: center;
}

.modal-content {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
    width: 80%;
    max-width: 500px;
    text-align: center;
}

.title {
    font-family: Georgia, serif;
    font-size: 3vw;
    text-align: center;
}

.linea {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
}

.blu-button, .blu-button2 {
    border: none;
    color: white;
    background-color: rgb(91, 91, 219);
    border-radius: 50%;
    width: 40px;
    height: 40px;
    cursor: pointer;
    font-size: 1.5rem;
}

.blu-button:hover, .blu-button2:hover {
    background-color: rgb(61, 61, 189);
}

.redButton, .backButton {
    border: none;
    color: white;
    background-color: rgb(255, 0, 0);
    border-radius: 10px;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 1rem;
}

.redButton {
    margin-left: 20vw;
}

.redButton:hover, .backButton:hover {
    background-color: rgb(173, 34, 34);
}

.redButton:focus, .backButton:focus {
    background-color: rgb(113, 15, 15);
}

.ListName {
    display: flex;
    align-items: center;
    padding: 10px;
    margin: 10px 0;
    background-color: #f9f9f9;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    cursor: pointer;
}

.ListName:hover {
    background-color: #e9e9e9;
}

.SubmitButton, .CloseButton {
    border: none;
    color: white;
    background-color: rgb(41, 160, 57);
    border-radius: 5px;
    padding: 10px 20px;
    cursor: pointer;
    margin: 10px;
}

.CloseButton {
    background-color: rgb(255, 0, 0);
}

.SubmitButton:hover, .CloseButton:hover {
    background-color: rgb(31, 130, 47);
}

.CloseButton:hover {
    background-color: rgb(173, 34, 34);
}

.SubmitButton:disabled {
    background-color: rgb(150, 150, 150);
    cursor: not-allowed;
}

.TextPopUp {
    margin-bottom: 20px;
    font-size: 1.2rem;
    text-align: center;
}

h1 {
    margin-bottom: 20px;
    font-size: 2rem;
    text-align: center;
}

.blu-button2 {
    margin-bottom: 20px;
}

.backButton {
    align-self: center;
}
</style>
