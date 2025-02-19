<template>
  <div id="appz">
    <textarea readonly v-model="code"></textarea>
    <!--    <div class="actions">-->
    <!--&lt;!&ndash;      <div class="action">&ndash;&gt;-->
    <!--&lt;!&ndash;        <a target="_blank" rel="noreferrer" @click="ipcHandle">Send IPC</a>&ndash;&gt;-->
    <!--&lt;!&ndash;      </div>&ndash;&gt;-->
    <!--    </div>-->
    <!--    <Versions />-->
    <div class="actions">
      <div @click="ugly" class="action">
        <a>
          Obfuscate
        </a>
      </div>
      <div @click="openfile" class="action">
        <a>
          Open file
        </a>
      </div>
      <div class="action">
        <select v-model="mode">
          <option value="0"> Best obfuscate </option>
          <option value="1"> Safe obfuscate </option>
        </select>
      </div>
    </div>
    <div class="txt-center" @click="about">
      Owl script guard by A1Gard - xStack team
    </div>
  </div>
</template>

<script>
import Versions from './components/Versions.vue'

export default {
  name: 'appz',
  components: {
    Versions
  },
  data: () => {
    return {
      mode: 0,
      code: `const mainWindow = new BrowserWindow({
    width: 900,
    height: 670,
    show: false,
    autoHideMenuBar: true,
    ...(process.platform === 'linux' ? { icon } : {}),
    webPreferences: {
      preload: join(__dirname, '../preload/index.js'),
      sandbox: false
    }
  })`
    }
  },
  props: {},
  mounted() {
    window.electron.ipcRenderer.on('ugly-result', (event, data) => {
      this.code = data
    })

    window.electron.ipcRenderer.on('ugly-error', (event, data) => {
      console.error('Error minifying code:', data)
    })
    window.electron.ipcRenderer.on('filedata', (event, data) => {
      this.code = data;
    })
  },
  computed: {},
  methods: {
    ipcHandle() {
      window.electron.ipcRenderer.send('ping')
    },
    ugly() {
      // this.code = terser.minify(this.code);
      window.electron.ipcRenderer.send('ugly', this.code, this.mode)
    },
    openfile() {
      window.electron.ipcRenderer.send('openfile')
    },
    about(){
      window.electron.ipcRenderer.send('link')
    }
  }
}
</script>

<style scoped>
#appz {
  width: 100%;
  padding: 0 2rem;
}

textarea {
  background: transparent;
  border: 1px solid #333333;
  border-radius: 3px;
  display: block;
  width: 100%;
  color: var(--ev-c-text-1);
  min-height: calc(100vh - 150px);
  font-size: 18px;
}

textarea:focus {
  outline: none;
  border: 1px solid #743b3b;
}
.txt-center{
  text-align: center;
  cursor: pointer;
}
</style>
