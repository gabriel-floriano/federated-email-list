<template>
  <div>
    <table>
      <thead>
        <tr>
          <th>
            <input type="checkbox" v-model="allSelected" />
          </th>
          <th>Name</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="email in emailsComputed"
          :key="email.address"
          :class="{ selected: email.selected }"
        >
          <td>
            <input
              type="checkbox"
              :id="email.address"
              :checked="email.selected"
              @change="() => toggleEmail(email)"
            />
          </td>
          <td>{{ email.name }}</td>
          <td>{{ email.address }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { computed } from "vue";
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  emails: { type: Array },
});

const emit = defineEmits([
  "update:emails",
  "email-selected-changed",
  "all-selected-emails-changed",
]);

const emailsComputed = computed({
  get() {
    return props.emails;
  },
  set(newEmails) {
    emit("update:emails", newEmails);
  },
});

const allSelected = computed({
  get() {
    return (
      emailsComputed.value.length > 0 &&
      emailsComputed.value.every((email) => email.selected)
    );
  },
  set() {
    toggleAllEmails();
  },
});

const toggleEmail = (email) => {
  email.selected = !email.selected;
  emit("email-selected-changed", email);
  emailsComputed.value = [...emailsComputed.value];
};

const toggleAllEmails = () => {
  const newValue = !allSelected.value;
  emit("all-selected-emails-changed", newValue);
};
</script>
