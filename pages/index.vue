<template>
  <!-- Contact List-->
   <div class="w-full px-2">
    <!-- Item Contact-->
     <ListItem v-for="contact in contacts" :key="contact.id">

      {{ contact.name }}

      <template #actions>
        <button @click="editContact(contact.id)">Edit</button>
        <button @click="removeContact(contact.id)">Remove</button>
      </template>
     </ListItem>
   </div>

  <UIButton @click="addContact"> Add Contact</UIButton>
    
</template>

<script setup>
  const router = useRouter();
  const { contactList, deleteContact } = useContacts()
  const contacts = contactList()

  useHead({
    title: "Contatinho.com",
    meta: [
      { name: "description", content: "Contact List Application" },
      // x social media
      {
        name: 'x-title', content: "Contatinho.com",
      },
      //General SEO
      {
        property: "og:title", content: "Contatinho.com"
      }
    ]
  })
  
  const addContact = () => {
    router.push("/form");
  }

  const editContact = (id) => {
    router.push({ path: '/form', query: { id }});
  }

  const removeContact = (id) => {
    deleteContact(id)
  }


</script>