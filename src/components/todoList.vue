<template>
  <section class="section">
    <div class="row align-items-top">
      <div class="col-lg-3">
        <div class="card mb-3 h-100 back-gray-custom p-5">
          <div class="card-tittle">
            <h2><b>To do vue Js</b></h2>
          </div>
          <br />
          <div class="mb-3">
            <input
              type="text"
              v-model="searchTerm"
              placeholder="Rechercher"
              class="form-control"
            />
          </div>
          <br />
          <div class="blue-pr">
            <h5><b>Favoris</b></h5>
          </div>
          <div>
            <ul class="custom-list ctl">
              <li>Mes tâches aujourd'hui</li>
              <li>Important <span class="badge bg-danger">1</span></li>
              <li>Personnel</li>
              <li>Toutes les tâches</li>
              <li>Términés</li>
            </ul>
          </div>
          <br />
          <div class="blue-pr d-flex align-items-center">
            <h5><b>Tags</b></h5>
            <button class="btn btn-info">
              <i class="bi bi-plus"></i>
            </button>
          </div>
          <div>
            <ul class="custom-list ctl">
              <li v-for="(tag, index) in tags" :key="index">
                <input type="radio" :checked="index % 2 === 0" />
                {{ tag["name"] }}
              </li>
            </ul>
          </div>
        </div>
      </div>

      <div class="col-lg-6 card">
        <div class="container" style="margin-top: 2%">
          <div class="row">
            <div class="col-md-10">
              <ul class="custom-list">
                <li><b>Aujourd'hui</b></li>
                <li class="blue-pr"><b>21 Mai 2024</b></li>
              </ul>
            </div>
            <div class="col-md-2 text-end">
              <button class="btn br-pr" @click="showModal = true">
                + Creer
              </button>
            </div>
          </div>
          <div style="margin-top: 5%">
            <div
              v-for="company in filteredAndPaginatedCompanies.data"
              :key="company['task']"
            >
              <div class="card-body">
                <div class="row">
                  <div class="col-md-1 text-center card-title">
                    <input type="checkbox" :checked="company['end-task']" />
                  </div>
                  <div class="col-md-10">
                    <h5 class="card-title">{{ company["task"] }}</h5>
                    <span><input type="radio" checked /></span>
                    <span class="text-muted small pt-1 fw-bold"
                      ><b> Devs - </b></span
                    >
                    <span class="text-info small pt-2 ps-1">{{
                      company["date"]
                    }}</span>
                  </div>
                  <div class="col-md-1 text-center card-title">
                    <i v-if="company['important']" class="bi bi-star-fill"></i>
                    <i v-if="!company['important']" class="bi bi-star"></i>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <nav aria-label="...">
            <ul class="pagination">
              <li class="page-item" :class="{ disabled: currentPage === 1 }">
                <a class="page-link" href="#" @click="prevPage">Precedent</a>
              </li>
              <li
                v-for="n in filteredAndPaginatedCompanies.totalPages"
                :key="n"
                class="page-item"
              >
                <a
                  class="page-link"
                  :class="{ active: currentPage === n }"
                  @click="setPage(n)"
                >
                  {{ n }}
                </a>
              </li>
              <li
                class="page-item"
                :class="{
                  disabled:
                    currentPage === filteredAndPaginatedCompanies.totalPages,
                }"
              >
                <a class="page-link" href="#" @click="nextPage">Suivant</a>
              </li>
            </ul>
          </nav>
        </div>
      </div>

      <div class="col-lg-3">
        <section class="section dashboard">
          <div class="card">
            <div class="card-body pb-0">
              <br />
              <div class="news">
                <div class="post-item clearfix text-center">
                  <img
                    src="../assets/img/profile-img.jpg"
                    alt=""
                    class="rounded-circle"
                  />
                  <h4><a href="#">John Die</a></h4>
                  <p>johndie@gmail.com</p>
                </div>
                <br />
                <div class="blue-pr">
                  <h5><b>Tâches terminées</b></h5>
                </div>
                <div>
                  <ul class="custom-list ctl">
                    <li v-for="list in companies">
                      <span
                        v-if="list['end-task'] === true"
                        class="text-muted small pt-1 fw-bold"
                        ><b> Devs - </b></span
                      >
                      <span
                        v-if="list['end-task'] === true"
                        class="text-info small pt-2 ps-1"
                        >{{ list["task"] }}</span
                      >
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>
  </section>
  <div class="card" v-if="showModal">
    <div class="card-body">
      <div
        class="modal fade"
        :class="{ show: showModal }"
        :style="{ display: showModal ? 'block' : 'none' }"
        id="verticalycentered"
        tabindex="-1"
      >
        <div class="modal-dialog modal-dialog-centered">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">creer une taches</h5>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
                @click="showModal = false"
              ></button>
            </div>
            <div class="modal-body">
              <form>
                <div class="row mb-3">
                  <label for="inputText" class="col-sm-2 col-form-label"
                    >Date</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="text"
                      class="form-control"
                      v-model="tachedetails.date"
                    />
                  </div>
                </div>
                <div class="row mb-3">
                  <label for="inputEmail" class="col-sm-2 col-form-label"
                    >Nom</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="name"
                      class="form-control"
                      v-model="tachedetails.task"
                    />
                  </div>
                </div>
                <div class="row mb-3">
                  <label for="inputPassword" class="col-sm-2 col-form-label"
                    >Tag</label
                  >
                  <div class="col-sm-10">
                    <input
                      type="text"
                      class="form-control"
                      v-model="tachedetails.tags"
                    />
                  </div>
                </div>
              </form>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-secondary"
                data-bs-dismiss="modal"
                @click="showModal = false"
              >
                Fermer
              </button>
              <button
                type="button"
                class="btn btn-primary"
                @click="insertTaches()"
              >
                Submit
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import axios from "axios";
import { ref, onMounted, computed } from "vue";

const companies = ref<any[]>([]);
const tags = ref<any[]>([]);
const searchTerm = ref("");
const currentPage = ref(1);
const itemsPerPage = 8;
const showModal = ref(false);

onMounted(async () => {
  try {
    const response = await axios.get(
      "https://retoolapi.dev/9lwmoL/to-do-edisys"
    );
    companies.value = response.data;
    const tagsList = await axios.get("https://retoolapi.dev/kEbZ3j/tags");
    tags.value = tagsList.data;
  } catch (error) {
    console.error("Une erreur s'est produite:", error);
  }
});

const filteredAndPaginatedCompanies = computed<{
  totalPages: number;
  data: any[];
}>(() => {
  const filtered = companies.value.filter(
    (company) =>
      company["task"] &&
      company["task"].toLowerCase().includes(searchTerm.value.toLowerCase())
  );

  if (filtered.length === 0) {
    return {
      totalPages: 0,
      data: [],
    };
  }

  const startIndex = (currentPage.value - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;

  const totalPages = Math.ceil(filtered.length / itemsPerPage);
  const data = filtered.slice(startIndex, endIndex);

  return {
    totalPages,
    data,
    showModal,
  };
});

const tachedetails = ref<any>({
  date: "Invalid date",
  tags: "",
  task: "",
  "end-task": false,
  important: false,
});
import router from "@/router";
const insertTaches = async () => {
  try {
    await axios.post(
      `https://retoolapi.dev/9lwmoL/to-do-edisys`,
      tachedetails.value
    );
    router.push(`/`);
  } catch {}
};

function prevPage() {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
}

function nextPage() {
  if (currentPage.value < filteredAndPaginatedCompanies.value.totalPages) {
    currentPage.value++;
  }
}

function setPage(pageNumber: number) {
  currentPage.value = pageNumber;
}
</script>

<style>
.line-with-text {
  display: flex;
  align-items: center;
  text-align: center;
}
.line {
  flex-grow: 1;
  border-bottom: 1px solid black;
}
.back-gray {
  background-color: #b5b9c4;
}

.blue-pr {
  color: rgb(14, 168, 220);
}

.br-pr {
  background-color: rgb(14, 168, 220);
}

.br-pr:hover,
.br-pr:focus {
  background-color: rgb(14, 168, 220);
}

.custom-list {
  list-style-type: none;
  padding-left: 0;
}

.back-gray-custom {
  background-color: #d7dae4;
}

.blue-pr button {
  margin-left: 70%;
}
</style>
