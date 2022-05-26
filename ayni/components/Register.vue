<template>
    <div class="max-w-sm w-full lg:max-w-full">
        <div class="mb-20 rounded border-r border-b border-l border-gray-400 lg:border-l-0 lg:border-t lg:border-gray-400 bg-red-200 rounded-b lg:rounded-b-none lg:rounded-r p-4 flex flex-col justify-between leading-normal"> 
            
            <div class="flex justify-center">
                
                <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
                    <h3 class="text-center not-italic text-xl mr-10 mb-1 bold">Registro de voluntario</h3>
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-bold mb-2" for="username">Primer nombre</label>
                        <input v-model="registro.nombre" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="Nombre">
                    </div>
                    
                        <label class="block text-gray-700 text-sm font-bold mb-2" for="apellido">Apellido</label>
                        <input v-model="registro.apellido" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"  type="text" placeholder="Apellido">
                        
                        <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Email</label>
                        <input v-model="registro.correo" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"  type="email" placeholder="Correo electronico">
                        
                        <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Dirección</label>
                        <input v-model="registro.direccion" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" placeholder="Direccion">
                        
                        <div id = "map"></div>

                        <label class="block text-gray-700 text-sm font-bold mb-2" for="password">RUT</label>
                        <input v-model="registro.rut" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" placeholder="RUT">

                        <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Estado</label>
                        <input v-model="registro.estado" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" placeholder="Estado de salud">
                    
                    <div class="mb-6">
                        <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Contraseña</label>
                        <input v-model="registro.contraseña" class="shadow appearance-none border border-red-500 rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"  type="password" placeholder="******************">
                        
                        <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Repita la contraseña</label>
                        <input v-model="registro.confirmar_contraseña" class="shadow appearance-none border border-red-500 rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" type="password" placeholder="******************">
                        <p class="text-red-500 text-xs italic">Las contraseñas deben coincidir</p>
                    </div>

                    <div class="flex items-center justify-between">
                        <button @click="send" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
                            Registrar
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>



<script>
// Importaciones para usar leaflet
import "leaflet/dist/leaflet"; 
import "leaflet/dist/leaflet.css";
var icon = require("leaflet/dist/images/marker-icon.png"); 
var LeafIcon = L.Icon.extend({
  options: { iconSize: [25, 41], iconAnchor: [12, 41], popupAnchor: [-3, -41] },
});
var myIcon = new LeafIcon({ iconUrl: icon });
export default {
    name: "MainPage",
    data() {
        return {
            registro: {},
            latitude: null,
            longitude: null
        };
    },
    methods: {
        send()
        {
            alert(this.latitude +" "+this.longitude)
            let newPoint = { 
                name: this.name,
                latitude: this.latitude,
                longitude: this.longitude,
                cod_regi: this.cod_regi
            };
            if (this.registro.contraseña !== this.registro.confirmar_contraseña){
                alert("Las contraseñas deben coincidir")
            }else 
            {
                this.$emit('data', {nombre:this.registro.nombre, apellido: this.registro.apellido,
                correo:this.registro.correo,direccion:this.registro.direccion,
                rut:this.registro.rut, estado:this.registro.estado,contraseña:this.registro.contraseña})
            }
        }
    },
    mounted: async function(){
        let _this = this
        this.mymap = L.map("map").setView([-33.456, -70.648], 7);
        //Se definen los mapas de bits de OSM
        L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
        attribution:
            '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
        maxZoom: 15,
        }).addTo(this.mymap);

        this.mymap.on("click", function (e) {
            _this.latitude = e.latlng.lat;
            _this.longitude = e.latlng.lng;
            console.log(e)
        });
        
    }
}
</script>

<style>
    #map {
        width: 400px;
        aspect-ratio: 16/9;
    }
</style>