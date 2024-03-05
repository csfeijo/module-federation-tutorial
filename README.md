# module-federation-tutorial


1) Create host:

mkdir module-federation-tutorial

npm create vite@latest react-app-host -- --template react
cd react-app=host
yarn

npm create vite@latest react-app-host -- --template react
cd react-app=host
yarn

2) Adicionar a dependencia do Module Federation para o Vite (ambos projetos)
yarn add @originjs/vite-plugin-federation

3) Vamos forçar o Vite do REMOTE a sempre carregar o server na mesma porta em DEV
```
  "scripts": {
    "dev": "vite --port 5001 --strictPort",
    "start": "vite --port 5001 --strictPort",
    "build": "vite build",
    "lint": "eslint . --ext js,jsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview --port 5001 --strictPort"
  },
```

4) Criar o componente de HEADER no REMOTE
* Em index.css de ambas app's:
```
Removidas:
body {
  //place-items: center;
  //display: flex;
}
```

* Em App.css de ambas app's:
```
Removidas:
::root {
  //max-width: 1280px;
  //padding: 2rem;
}
```
5) Hora de configurar o module federation do REMOTE
* Dentro do vite.config.js do REMOTE:
```
plugins: [
    react(),
    federation({
      name: 'remote_app',
      filename: 'remoteEntry.js',
      exposes: {
        './Header': './src/components/Header'
      },
      shared: ['react', 'react-dom']
    })
  ],
  build: {
    modulePreload: false,
    target: 'esnext',
    minify: false,
    cssCodeSplit: false
  }
```
Agora roda o build e preview:
```
yarn build
yarn preview
```

6) Agora configura o HOST para importar o componente via Module Federation
* Dentro do vite.config.js do HOST:
```
plugins: [
    react(),
    federation({
      name: 'host_app',
      remotes: {
        remoteApp: 'http://localhost:5001/assets/remoteEntry.js'
      }
    })
  ],
  build: {
    modulePreload: false,
    target: 'esnext',
    minify: false,
    cssCodeSplit: false
  }
```
