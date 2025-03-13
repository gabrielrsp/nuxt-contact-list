<template>
  <div>

    <Title>Edit contact info {{ contact.name }} </Title>
    <Meta  name="description" :content="`Edit contact info ${contact.name}`" />

    <Meta  property="x:title" :content="`Edit contact info ${contact.name}`" />

    <form @submit="saveContact">
      <UIInput label="Name" type="text" v-model="contact.name" />
      
      <UIInput label="Email" type="email" v-model="contact.email" />
      
      <UIInput label="Phone" type="text" v-model="contact.phone" />
      
      <UIButton type="submit">Save Contact</UIButton>
    </form>

    </div>
</template>

<script setup>
const { createContact, updateContact,  getContact } = useContacts()

const router = useRouter()
const route = useRoute()

const contact = reactive({
  id: '',
  name: '',
  email: '',
  phone: ''
})

onMounted(() => {
 const id = route.query.id

 if(id) {
  const { name, email, phone } = getContact(id)

  contact.id = id
  contact.name = name
  contact.email = email
  contact.phone = phone
 } 
})

const saveContact = (event) => {
  event.preventDefault() 


  if(contact.id) {
    updateContact(contact.id, contact)
  } else {
    createContact(contact)
  }
  router.push("/")
}

</script>