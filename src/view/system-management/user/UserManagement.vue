<template>
  <div class="grid">
    <div class="col-12">
      <div class="card">
        <div class="content">
            <Accordion  class="mb-4">
              <AccordionTab :header="t('advancedSearch')">
                  <div class="p-fluid">
                    <div class="formgrid grid">
                      <div class="field col">
                        <label for="username">{{ this.$t("user.properties.username") }}</label>
                        <inputText id="username" type="text"/>
                      </div>
                      <div class="field col">
                        <label for="fullname">{{ this.$t("user.properties.fullname") }}</label>
                        <inputText id="fullname" type="text"/>
                      </div>
                    </div>
                    <div class="formgrid grid">
                      <div class="field col">
                        <label for="email">{{ this.$t("user.properties.email") }}</label>
                        <inputText id="email" type="text"/>
                      </div>
                      <div class="field col">
                        <label for="searchStatus">{{ this.$t("user.properties.status") }}</label>
                        <Dropdown
                            id="searchStatus"
                            v-model="statusOption"
                            optionLabel="status"
                            :options="statusOptions"
                            :placeholder="t('select')"></Dropdown>
                      </div>
                    </div>
                  </div>
                  <div class="grid justify-content-center mt-5">
                    <Button type="button" class="ml-3" :label="t('search')" icon="pi pi-search"/>
                  </div>
              </AccordionTab>
            </Accordion>

            <toolbar class="mb-4">
              <template v-slot:start>
                <div class="my-2">
                  <Button type="button" :label="t('create')" class="p-button-success mr-2" icon="pi pi-plus" @click="openNew1"/>
                  <Button type="button" :label="t('delete')" class="p-button-danger" icon="pi pi-trash"/>
                </div>
              </template>
              <template v-slot:end>
                <div class="my-2">
                  <Button type="button" :label="t('import')" class="inline-block mr-2" icon="pi pi-plus"/>
                  <Button type="button" :label="t('export')" class="p-button-help" icon="pi pi-upload"/>
                </div>
              </template>
            </toolbar>
          </div>

<!--        <DataTable ref="dt" value="products" selection="selectedProducts" dataKey="id" paginator="true" rows="10" filters="filters"-->
<!--                   paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown" :rowsPerPageOptions="[5,10,25]"-->
<!--                   currentPageReportTemplate="Showing {first} to {last} of {totalRecords} products" responsiveLayout="scroll">-->
<!--          <template #header>-->
<!--            <div class="flex flex-column md:flex-row md:justify-content-between md:align-items-center">-->
<!--              <h5 class="m-0">{{ $t('components.UserManagement') }}</h5>-->
<!--              <span class="block mt-2 md:mt-0 p-input-icon-left">-->
<!--                                <i class="pi pi-search" />-->
<!--                                <InputText :placeholder="t('user.searchText')" />-->
<!--                            </span>-->
<!--            </div>-->
<!--          </template>-->

<!--          <Column selectionMode="multiple" headerStyle="width: 3rem"></Column>-->
<!--          <Column field="fullname" :header="t('user.properties.fullname')" :sortable="true" headerStyle="min-width:10rem;">-->
<!--            <template #body="slotProps">-->
<!--              <span class="p-column-title"></span>-->
<!--              {{slotProps.data.code}}-->
<!--            </template>-->
<!--          </Column>-->

<!--          <Column field="name" header="Name" :sortable="true" headerStyle="min-width:10rem;">-->
<!--            <template #body="slotProps">-->
<!--              <span class="p-column-title">Name</span>-->
<!--              {{slotProps.data.name}}-->
<!--            </template>-->
<!--          </Column>-->

<!--          <Column field="username" :header="t('user.properties.username')" :sortable="true" headerStyle="min-width:10rem;">-->
<!--            <template #body="slotProps">-->
<!--              <span class="p-column-title">Tên đăng nhập</span>-->
<!--              {{slotProps.data.name}}-->
<!--            </template>-->
<!--          </Column>-->
<!--          <Column field="email" :header="t('user.properties.email')" :sortable="true" headerStyle="min-width:8rem;">-->
<!--            <template #body="slotProps">-->
<!--              <span class="p-column-title">Hộp thư</span>-->
<!--            </template>-->
<!--          </Column>-->
<!--          <Column field="status" :header="t('user.properties.status')" :sortable="true" headerStyle="min-width:10rem;">-->
<!--            <template #body="slotProps">-->
<!--              <span class="p-column-title">Status</span>-->
<!--              <span :class="'product-badge status-' + (slotProps.data.inventoryStatus ? slotProps.data.inventoryStatus.toLowerCase() : '')">{{slotProps.data.inventoryStatus}}</span>-->
<!--            </template>-->
<!--          </Column>-->
<!--        </DataTable>-->


      <!--        UserTable-->

      <DataTable ref="dt" :value="users" v-model:selection="selectedUsers" dataKey="id" :paginator="true" :rows="10" :filters="filtersUser"
                 paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown" :rowsPerPageOptions="[5,10,25]"
                 currentPageReportTemplate="Showing {first} to {last} of {totalRecords} products" responsiveLayout="scroll">
        <template #header>
          <div class="flex flex-column md:flex-row md:justify-content-between md:align-items-center">
            <h5 class="m-0">{{ $t('components.UserManagement') }}</h5>
            <span class="block mt-2 md:mt-0 p-input-icon-left">
                                <i class="pi pi-search" />
                                <InputText v-model="filtersUser['global'].value" :placeholder="t('user.searchText')" />
                            </span>
          </div>
        </template>

        <Column selectionMode="multiple" headerStyle="width: 3rem"></Column>
        <Column field="code" :header="t('user.properties.fullname')" :sortable="true" headerStyle="min-width:10rem;">
          <template #body="slotProps">
            <span class="p-column-title">Họ và tên</span>
            {{slotProps.data.code}}
          </template>
        </Column>
        <Column field="name" :header="t('user.properties.username')" :sortable="true" headerStyle="min-width:10rem;">
          <template #body="slotProps">
            <span class="p-column-title">Tên đăng nhập</span>
            {{slotProps.data.name}}
          </template>
        </Column>
        <Column field="price" :header="t('user.properties.email')" :sortable="true" headerStyle="min-width:10rem;">
          <template #body="slotProps">
            <span class="p-column-title">Hộp thư</span>
            {{formatCurrency(slotProps.data.price)}}
          </template>
        </Column>
        <Column field="inventoryStatus" :header="t('user.properties.status')" :sortable="true" headerStyle="min-width:8rem;">
          <template #body="slotProps">
            <span class="p-column-title">Status</span>
            <span :class="'product-badge status-' + (slotProps.data.inventoryStatus ? slotProps.data.inventoryStatus.toLowerCase() : '')">{{slotProps.data.inventoryStatus}}</span>
          </template>
        </Column>
        <Column headerStyle="min-width:10rem;">
          <template #body="slotProps">
            <Button icon="pi pi-pencil" class="p-button-rounded p-button-success mr-2" @click="editProduct(slotProps.data)" />
            <Button icon="pi pi-trash" class="p-button-rounded p-button-warning mt-2" @click="confirmDeleteProduct(slotProps.data)" />
          </template>
        </Column>
      </DataTable>

        <!--        Sua-->
        <Dialog v-model:visible="productDialog" :style="{width: '450px'}" header="Product Details" :modal="true" class="p-fluid">
<!--          <img :src="'images/product/' + product.image" :alt="product.image" v-if="product.image" width="150" class="mt-0 mx-auto mb-5 block shadow-2" />-->
          <div class="field">
            <label for="name">Name</label>
            <InputText id="name" v-model.trim="product.name" required="true" autofocus :class="{'p-invalid': submitted && !product.name}" />
            <small class="p-invalid" v-if="submitted && !product.name">Name is required.</small>
          </div>
          <div class="field">
            <label for="name">Name</label>
            <InputText id="name" v-model.trim="product.name" required="true" autofocus :class="{'p-invalid': submitted && !product.name}" />
            <small class="p-invalid" v-if="submitted && !product.name">Name is required.</small>
          </div>
          <div class="field">
            <label for="inventoryStatus" class="mb-3">Inventory Status</label>
            <Dropdown id="inventoryStatus" v-model="product.inventoryStatus" :options="statuses" optionLabel="label" placeholder="Select a Status">
              <template #value="slotProps">
                <div v-if="slotProps.value && slotProps.value.value">
                  <span :class="'product-badge status-' +slotProps.value.value">{{slotProps.value.label}}</span>
                </div>
                <div v-else-if="slotProps.value && !slotProps.value.value">
                  <span :class="'product-badge status-' +slotProps.value.toLowerCase()">{{slotProps.value}}</span>
                </div>
                <span v-else>
									{{slotProps.placeholder}}
								</span>
              </template>
            </Dropdown>
          </div>
          <div class="formgrid grid">
            <div class="field col">
              <label for="price">Email</label>
              <InputNumber id="price" v-model="product.price" mode="currency" currency="USD" locale="en-US" />
            </div>
          </div>
          <template #footer>
            <Button label="Cancel" icon="pi pi-times" class="p-button-text" @click="hideDialog"/>
            <Button label="Save" icon="pi pi-check" class="p-button-text" @click="saveProduct" />
          </template>
        </Dialog>

        <!--        Xoa-->
        <Dialog v-model:visible="deleteProductDialog" header="Confirm" modal="true">
          <div class="flex align-items-center justify-content-center">
            <i class="pi pi-exclamation-triangle mr-3" style="font-size: 2rem" />
            <span v-if="user">Are you sure you want to delete <b>{{user.name}}</b>?</span>
          </div>
          <template>
            <Button label="No" icon="pi pi-times" class="p-button-text" />
            <Button label="Yes" icon="pi pi-check" class="p-button-text"  />
          </template>
        </Dialog>

      </div>
    </div>
  </div>

</template>

<script>
import {useI18n} from 'vue-i18n';
import {FilterMatchMode} from 'primevue/api';
import ProductService from '../../../service/ProductService';
import UserService from '../../../service/UsersService';

export default {
  setup() {
      const {t} = useI18n();
      return {t}
  },
  data() {
    return {
      statusOptions: [
        {status: 'Đang hoạt động', code: '1'},
        {status: 'Không hoạt động', code: '2'}
      ],
      statusOption: null,

      users: null,
      userDialog: false,
      deleteUserDialog: false,
      deleteUsersDialog: false,
      user: {},
      selectedUsers: null,
      filtersUser: {},
      submittedUser: false,
      statusesUser: [
        {label: 'INSTOCK', value: 'instock'},
        {label: 'LOWSTOCK', value: 'lowstock'},
        {label: 'OUTOFSTOCK', value: 'outofstock'}
      ]
    }
  },
  productService: null,
  created() {
    this.userService = new UserService();
    this.initFiltersUser();
  },
  mounted() {
    this.userService.getUser().then(data => this.users = data);
  },
  methods: {
    openNew1() {
      this.$router.push({
        name: 'CreateUser'
      })
    },
    formatCurrency(value) {
      if(value)
        return value.toLocaleString('en-US', {style: 'currency', currency: 'USD'});
      return;
    },
    openNew() {
      this.product = {};
      this.submitted = false;
      this.productDialog = true;

    },
    hideDialog() {
      this.productDialog = false;
      this.submitted = false;
    },
    saveProduct() {
      this.submitted = true;
      if (this.product.name.trim()) {
        if (this.product.id) {
          this.product.inventoryStatus = this.product.inventoryStatus.value ? this.product.inventoryStatus.value: this.product.inventoryStatus;
          this.products[this.findIndexById(this.product.id)] = this.product;
          this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Updated', life: 3000});
        }
        else {
          this.product.id = this.createId();
          this.product.code = this.createId();
          this.product.image = 'product-placeholder.svg';
          this.product.inventoryStatus = this.product.inventoryStatus ? this.product.inventoryStatus.value : 'INSTOCK';
          this.products.push(this.product);
          this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Created', life: 3000});
        }
        this.productDialog = false;
        this.product = {};
      }
    },
    editProduct(product) {
      this.product = {...product};
      this.productDialog = true;
    },
    confirmDeleteProduct(product) {
      this.product = product;
      this.deleteProductDialog = true;
    },
    deleteProduct() {
      this.products = this.products.filter(val => val.id !== this.product.id);
      this.deleteProductDialog = false;
      this.product = {};
      this.$toast.add({severity:'success', summary: 'Successful', detail: 'Product Deleted', life: 3000});
    },
    findIndexById(id) {
      let index = -1;
      for (let i = 0; i < this.products.length; i++) {
        if (this.products[i].id === id) {
          index = i;
          break;
        }
      }
      return index;
    },
    createId() {
      let id = '';
      var chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      for ( var i = 0; i < 5; i++ ) {
        id += chars.charAt(Math.floor(Math.random() * chars.length));
      }
      return id;
    },
    exportCSV() {
      this.$refs.dt.exportCSV();
    },
    confirmDeleteSelected() {
      this.deleteProductsDialog = true;
    },
    deleteSelectedProducts() {
      this.products = this.products.filter(val => !this.selectedProducts.includes(val));
      this.deleteProductsDialog = false;
      this.selectedProducts = null;
      this.$toast.add({severity:'success', summary: 'Successful', detail: 'Products Deleted', life: 3000});
    },
    initFilters() {
      this.filters = {
        'global': {value: null, matchMode: FilterMatchMode.CONTAINS},
      }
    },
    initFiltersUser() {
      this.filtersUser = {
        'global': {value: null, matchMode: FilterMatchMode.CONTAINS},
      }
    }
  }
}
</script>

<style scoped lang="scss">
</style>
