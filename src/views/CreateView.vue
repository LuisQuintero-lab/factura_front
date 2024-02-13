<template>
  <h1>Create CFDI</h1>
  <v-sheet width="auto" class="mx-auto">
    <v-form ref="form">
        <v-row>
            <v-col cols="12" sm="6">
                <!-- Receptor -->
                <v-select
                    v-model="receptor"
                    :items="receptorOptions"
                    :rules="[v => !!v || 'Receptor is required']"
                    label="Receptor"
                    item-title="RazonSocial"
                    item-value="UID"
                    required
                ></v-select>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Tipo de documento -->
                <v-select
                    v-model="tipoDocumento"
                    :items="tipoDocumentoOptions"
                    :rules="[v => !!v || 'Tipo de documento is required']"
                    label="Tipo de documento"
                    required
                ></v-select>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Lugar de expedición -->
                <v-text-field
                    v-model="lugarExpedicion"
                    label="Lugar de expedición"
                    required
                ></v-text-field>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Uso de CFDI -->
                <v-select
                    v-model="usoCFDI"
                    :items="usoCFDIOptions"
                    :rules="[v => !!v || 'Uso de CFDI is required']"
                    label="Uso de CFDI"
                    item-title="name"
                    item-value="key"
                    required
                ></v-select>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Serie -->
                <v-select
                    v-model="serie"
                    :items="serieOptions"
                    :rules="[v => !!v || 'Serie is required']"
                    label="Serie"
                    item-title="SerieName"
                    item-value="SerieID"
                    required
                ></v-select>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Forma de pago -->
                <v-select
                    v-model="formaPago"
                    :items="formaPagoOptions"
                    :rules="[v => !!v || 'Forma de pago is required']"
                    label="Forma de pago"
                    item-title="name"
                    item-value="key"
                    required
                ></v-select>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Método de pago -->
                <v-select
                    v-model="metodoPago"
                    :items="metodoPagoOptions"
                    :rules="[v => !!v || 'Método de pago is required']"
                    label="Método de pago"
                    required
                ></v-select>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Moneda -->
                <v-select
                    v-model="moneda"
                    :items="monedaOptions"
                    :rules="[v => !!v || 'Moneda is required']"
                    label="Moneda"
                    required
                ></v-select>
            </v-col>

            <v-col cols="12" sm="6">
                <!-- Enviar por correo -->
                <v-checkbox
                    v-model="enviarPorCorreo"
                    label="Enviar por correo"
                ></v-checkbox>
            </v-col>
        </v-row>

      <!-- Conceptos -->
      <v-card v-for="(concept, index) in conceptos" :key="index" class="mb-4">
        <v-card-title>
            Concepto {{ index + 1 }}
            <v-btn v-if="!concept.expanded" @click="expandConcept(index)">Ver detalles</v-btn>
            <v-btn v-else @click="collapseConcept(index)">Ocultar detalles</v-btn>
        </v-card-title>
        <v-divider></v-divider>

        <v-row>
            <v-col cols="12" sm="6">
            <v-text-field v-model="concept.ClaveProdServ" label="Clave producto o servicio" required v-if="concept.expanded"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
            <v-text-field v-model="concept.Cantidad" label="Cantidad" required v-if="concept.expanded"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
            <v-text-field v-model="concept.ClaveUnidad" label="Clave de unidad" required v-if="concept.expanded"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
            <v-text-field v-model="concept.ValorUnitario" label="Valor unitario" required v-if="concept.expanded"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
            <v-text-field v-model="concept.Descripcion" label="Descripción" required v-if="concept.expanded"></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
            <v-text-field v-model="concept.ObjetoImp" label="Objeto de impuesto" required v-if="concept.expanded"></v-text-field>
            </v-col>
            <!-- Modelo para IVA-->
            <!-- <v-col cols="12" sm="6">
            <v-text-field 
            v-model="concept.Iva" 
            label="IVA %" 
            readonly
            :style="{ color: 'grey' }"
            v-if="concept.expanded"></v-text-field>
            </v-col> -->
            <v-col cols="12" sm="6">
            <v-text-field 
            v-model="concept.total" 
            label="Total de concepto" 
            readonly 
            :style="{ color: 'grey' }"
            v-if="concept.expanded"
            ></v-text-field>
            </v-col>
        </v-row>
        <!-- Resumen del concepto -->
        <v-row v-if="!concept.expanded">
          <v-col cols="12">
            <v-subheader>Resumen</v-subheader>
            <v-list dense>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Descripción:</v-list-item-title>
                  <v-list-item-subtitle>{{ concept.Descripcion }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Cantidad:</v-list-item-title>
                  <v-list-item-subtitle>{{ concept.Cantidad }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Valor Unitario:</v-list-item-title>
                  <v-list-item-subtitle>{{ concept.ValorUnitario }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Total de concepto:</v-list-item-title>
                  <v-list-item-subtitle>{{ concept.total }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-col>
        </v-row>

        <v-btn color="error" @click="removeConcept(index)">Remove Concepto</v-btn>
      </v-card>

      <v-btn color="primary" @click="addConcept">Agregar Concepto +</v-btn>
      <!-- Mensaje de error -->
      <br>
      <span v-if="showValidationError" class="error-message">Debe agregar al menos un concepto</span>

      <!-- Botones para acciones de form validate, reset, resetValidation-->
      <v-row>
        <v-col cols="4">  
            <v-btn
            color="success"
            class="mt-4"
            block
            @click="validate"
            >
            Crear CFDI
            </v-btn>

            <v-btn
            color="blue-grey-lighten-4"
            class="mt-4"
            block
            @click="reset"
            >
            Reset Form
            </v-btn>

            <v-btn
            color="blue-grey-lighten-4"
            class="mt-4"
            block
            @click="resetValidation"
            >
            Reset Validation
            </v-btn>
        </v-col>
      </v-row>
    </v-form>

    <v-bottom-sheet v-model="sheet" inset>
      <v-card class="text-center">
        <v-card-text>
          <v-btn variant="text" @click="sheet = false">
            Cerrar
          </v-btn>
          <br><br>
          <div>
            <pre>{{ apiResponseMessage }}</pre>
          </div>
        </v-card-text>
      </v-card>
    </v-bottom-sheet>
  </v-sheet>
</template>



<script>
import axios from 'axios';

axios.interceptors.request.use(config => {
    config.headers['Accept'] = 'application/json';
    return config;
});

export default {
  data: () => ({
    receptor: null,
    receptorOptions: [],
    tipoDocumento: null,
    tipoDocumentoOptions: [
      'factura',
      '----',
      '-----',
    ],
    lugarExpedicion: '',
    usoCFDI: null,
    usoCFDIOptions: [],
    serie: null,
    serieOptions: [],
    formaPago: null,
    formaPagoOptions: [],
    metodoPago: null,
    metodoPagoOptions: [
      'PUE',
      'PPD',
    ],
    moneda: null,
    monedaOptions: [
      'MXN',
    ],
    enviarPorCorreo: false,
    conceptos: [],
    showValidationError: false,
    sheet: false,
    apiResponse: '',
  }),

  created() {
    this.fetchSelectOptions('http://localhost:8000/invoice/clients', 'receptorOptions');
    this.fetchSelectOptions('http://localhost:8000/invoice/cfdiUse', 'usoCFDIOptions');
    this.fetchSelectOptions('http://localhost:8000/invoice/series', 'serieOptions');
    this.fetchSelectOptions('http://localhost:8000/invoice/payWay', 'formaPagoOptions');
  },

  methods: {
    async fetchSelectOptions(url, optionsProp) {
        try {
            const response = await axios.get(url);
            // Verifica si la respuesta contiene un array de objetos
            if (Array.isArray(response.data.data)) {
                // Asigna los datos al optionsProp
                this[optionsProp] = response.data.data;
            } else {
                this[optionsProp] = response.data;
            }
        } catch (error) {
            console.error('Error al obtener las opciones:', error);
        }
    },

    async validate () { 
      const { valid } = await this.$refs.form.validate()

      if (valid && this.conceptos.length > 0) {
        

        const formData = {
            Receptor: this.receptor ? { UID: this.receptor } : null,
            LugarExpedicion: this.lugarExpedicion,
            UsoCFDI: this.usoCFDI,
            Serie: this.serie,
            FormaPago: this.formaPago,
            MetodoPago: this.metodoPago,
            Moneda: this.moneda,
            EnviarCorreo: this.enviarPorCorreo,
            Conceptos: this.conceptos,
            TipoDocumento:  this.tipoDocumento,
        }
        try {
            const response = await axios.post('http://localhost:8000/invoice/store', formData);
          console.log('Respuesta del API:', response.data);

          if (response.data.response === 'success') {
              this.apiResponseMessage = 'CFDI Creado correctamente';
            } else {
              this.apiResponseMessage = 'Hubo un error al crear el CFDI';
          }

        
          this.sheet = true; // Mostrar el bottom sheet con la respuesta del API
          this.showValidationError = false;
        } catch (error) {
            console.error('Error Al timbrar el invoice:', error);
        } 
      } else {
        this.showValidationError = true; // Muestra el mensaje de error si no se cumple la validación
        console.error('Debe agregar al menos un concepto');
      }

    },
    addConcept() {
        // Colapsa todos los conceptos existentes
        this.conceptos.forEach(concept => {
            concept.expanded = false;
        });

        this.conceptos.push({
            ClaveProdServ: "",
            Cantidad: "",
            ClaveUnidad: "",
            ValorUnitario: "",
            Descripcion: "",
            ObjetoImp: "",
            Iva: 1,
            total: 0,
            expanded: true, // Concepto expandido por defecto
        });
    },
    removeConcept(index) {
      this.conceptos.splice(index, 1);
    },
    expandConcept(index) {
        this.conceptos[index].expanded = true;
    },
    collapseConcept(index) {
        this.conceptos[index].expanded = false;
    },
    reset () {
      this.$refs.form.reset()
    },
    resetValidation () {
      this.$refs.form.resetValidation()
    },
  },

  watch: {
  'conceptos': {
    deep: true,
    handler(newValue) {
      // Itera sobre cada concepto y actualiza el campo 'total'
      newValue.forEach(concepto => {
        // Verifica que haya un valor en Cantidad, ValorUnitario e Iva
        if (concepto.Cantidad && concepto.ValorUnitario && concepto.Iva) {
          const cantidad = parseFloat(concepto.Cantidad);
          const valorUnitario = parseFloat(concepto.ValorUnitario);
          // const iva = parseFloat(concepto.Iva) / 100; // Divide el IVA entre 100
          // const impuesto = iva * valorUnitario; // Calcula el impuesto
          const total = (valorUnitario) * cantidad; // Calcula el total
          concepto.total = total.toFixed(2);
        } else {
          concepto.total = 0;
        }
      });
    }
  }
}

}
</script>

<style>
.error-message {
  color: red; /* Establecer el color del texto en rojo */
}

.top-padding {
  padding-top: 20px;
}
</style>
