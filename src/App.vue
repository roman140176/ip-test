<script setup>
import { computed, ref } from "vue";

const USER_LIMIT = 6;

const userItems = ref([
  { id: 1, name: "Shoes 1" },
  { id: 2, name: "Shoes 2" },
  { id: 3, name: "Shoes 3" },
  { id: 4, name: "Shoes 4" },
  { id: 5, name: "T-shirt 1" },
  { id: 6, name: "T-shirt 2" },
  { id: 7, name: "T-shirt 3" },
  { id: 8, name: "T-shirt 4" }
]);

const optionItems = ref([
  { id: 11, name: "Jacket 1" },
  { id: 12, name: "Jacket 2" },
  { id: 13, name: "Jacket 3" },
  { id: 14, name: "Jacket 4" },
  { id: 15, name: "Hoodie 1" },
  { id: 16, name: "Hoodie 2" },
  { id: 17, name: "Hoodie 3" },
  { id: 18, name: "Hoodie 4" }
]);

const selectedUserIds = ref([]);
const selectedOptionId = ref(null);

const userById = computed(() => new Map(userItems.value.map((it) => [it.id, it])));
const optionById = computed(() => new Map(optionItems.value.map((it) => [it.id, it])));

const selectedUserItems = computed(() =>
  selectedUserIds.value.map((id) => userById.value.get(id)).filter(Boolean)
);

const selectedOptionItem = computed(() =>
  selectedOptionId.value == null ? null : optionById.value.get(selectedOptionId.value) ?? null
);

function toggleUserItem(itemId) {
  const index = selectedUserIds.value.indexOf(itemId);
  if (index !== -1) {
    selectedUserIds.value.splice(index, 1);
    return;
  }
  if (selectedUserIds.value.length >= USER_LIMIT) return;
  selectedUserIds.value.push(itemId);
}

function selectOptionItem(itemId) {
  selectedOptionId.value = itemId;
}

function userOrder(itemId) {
  const index = selectedUserIds.value.indexOf(itemId);
  return index === -1 ? null : index + 1;
}
</script>

<template>
  <main class="page">
    <section class="panel panel--top-left" aria-label="Selected user items">
      <div class="panel__content panel__content--top-left">
        <div class="strip">
          <button
            v-for="item in selectedUserItems"
            :key="item.id"
            type="button"
            class="item item--small item--selected"
            @click="toggleUserItem(item.id)"
            :title="`Remove ${item.name}`"
          >
            {{ item.name }}
          </button>
        </div>

        <div class="counter">selected: {{ selectedUserItems.length }} / {{ USER_LIMIT }}</div>
      </div>
    </section>

    <section class="panel panel--top-right" aria-label="Selected option item">
      <div class="panel__content panel__content--top-right">
        <div v-if="selectedOptionItem" class="selected-big">
          <div class="selected-big__label">SELECTED ITEM</div>
          <div class="selected-big__name">{{ selectedOptionItem.name }}</div>
        </div>
        <div v-else class="selected-big selected-big--placeholder">SELECTED ITEM</div>
      </div>
    </section>

    <section class="panel panel--bottom-left" aria-label="User items">
      <div class="panel__content">
        <div class="grid">
          <button
            v-for="item in userItems"
            :key="item.id"
            type="button"
            class="item"
            :class="{ 'item--selected': selectedUserIds.includes(item.id) }"
            @click="toggleUserItem(item.id)"
          >
            <span class="item__name">{{ item.name }}</span>
            <span v-if="userOrder(item.id)" class="item__badge">{{ userOrder(item.id) }}</span>
          </button>
        </div>
      </div>
    </section>

    <section class="panel panel--bottom-right" aria-label="Option items">
      <div class="panel__content">
        <div class="grid">
          <button
            v-for="item in optionItems"
            :key="item.id"
            type="button"
            class="item"
            :class="{ 'item--selected': selectedOptionId === item.id }"
            @click="selectOptionItem(item.id)"
          >
            <span class="item__name">{{ item.name }}</span>
          </button>
        </div>
      </div>
    </section>
  </main>
</template>

