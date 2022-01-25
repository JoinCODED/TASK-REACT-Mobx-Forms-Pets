### Bootstrap Modal

1. `npm install react-bootstrap bootstrap@5.1.3`
2. Create a file called `PetCreateModal.js` in `Components`
3. Setup your component and import `Modal` from bootstrap.

```javascript
import { Modal } from 'react-bootstrap';
```

4. Create a state for showing the modal with an intial value of `false`.

```javascript
const [show, setShow] = useState(false);
```

5. Create a `handleClose` and `handleShow` functions that changes our state to `false` or `true`.

```javascript
const [show, setShow] = useState(false);
const handleClose = () => setShow(false);
const handleShow = () => setShow(true);
```

6. Create a button that when pressed calls the `handleShow` function.

```javascript
<button type="button" class="btn btn-info" onClick={handleShow}>
  Add a pet
</button>
```

7. create your modal as the docs suggests [click here for the docs](https://react-bootstrap.github.io/components/modal/)

```javascript
<>
  <button type="button" class="btn btn-info" onClick={handleShow}>
    Add a pet
  </button>
  <Modal show={show} onHide={handleClose}>
    <Modal.Header closeButton>
      <Modal.Title>Add a pet</Modal.Title>
    </Modal.Header>
    <Modal.Body>Form goes here</Modal.Body>
    <Modal.Footer>
      <button type="button" class="btn btn-info" onClick={handleClose}>
        Close
      </button>
      <button type="button" class="btn btn-info" onClick={handleClose}>
        Add
      </button>
    </Modal.Footer>
  </Modal>
</>
```

### Bootstrap Form

1. In the Modal body create a form with the following fields: `name`,`type` and `image` docs are your friend! [click here for the docs](https://react-bootstrap.github.io/components/forms/)

```javascript
<Modal.Body>
  <Form>
    <Form.Group className="mb-3">
      <Form.Label>Pet name</Form.Label>
      <Form.Control type="text" placeholder="Pet name" />
    </Form.Group>
    <Form.Group className="mb-3">
      <Form.Label>Pet type</Form.Label>
      <Form.Control type="text" placeholder="Pet type" />
    </Form.Group>
    <Form.Group className="mb-3">
      <Form.Label>Pet image</Form.Label>
      <Form.Control type="text" placeholder="Pet image url" />
    </Form.Group>
  </Form>
</Modal.Body>
```

2. Create a state to hold our data.

```javascript
const [petForm, setPetForm] = useState({
  name: '',
  type: '',
  image: '',
});
```

3. Create a `handleChange` method that modifies our state object and pass it to every field in their `onChange` property. 

```javascript
const handleChange = (e) => {
  setPetForm({ ...petForm, [e.target.name]: e.target.value });
};
```

```javascript
            <Form.Group className="mb-3">
              <Form.Label>Pet name</Form.Label>
              <Form.Control
                onChange={handleChange}
                type="text"
                placeholder="Pet name"
              />
            </Form.Group>
            <Form.Group className="mb-3">
              <Form.Label>Pet type</Form.Label>
              <Form.Control
                onChange={handleChange}
                type="text"
                placeholder="Pet type"
              />
            </Form.Group>
            <Form.Group className="mb-3">
              <Form.Label>Pet image</Form.Label>
              <Form.Control
                onChange={handleChange}
                type="text"
                placeholder="Pet image url"
              />
            </Form.Group>
```
4. Add a `name` property to each field that matches the state object.
```javascript
            <Form.Group className="mb-3">
              <Form.Label>Pet name</Form.Label>
              <Form.Control
                onChange={handleChange}
                name="name"
                type="text"
                placeholder="Pet name"
              />
            </Form.Group>
            <Form.Group className="mb-3">
              <Form.Label>Pet type</Form.Label>
              <Form.Control
                onChange={handleChange}
                name="type"
                type="text"
                placeholder="Pet type"
              />
            </Form.Group>
            <Form.Group className="mb-3">
              <Form.Label>Pet image</Form.Label>
              <Form.Control
                onChange={handleChange}
                name="image"
                type="text"
                placeholder="Pet image url"
              />
            </Form.Group>
```
5. Create a `handleSubmit` method that for now console logs our state data and closes the modal. Also pass this method to your submit button.

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  console.log(pet);
  handleClose();
};
```

```javascript
<button type="button" class="btn btn-info" onClick={handleSubmit}>
  Add
</button>
```

6. Don't forget to prevent the page from refreshing.
7. Import our modal in `PetsList` and render it at the top.

```javascript
<PetCreateModal />
```

### Creating in mobx.

1. In our `petStore` let's create a method for adding a pet. Don't forget to mark it as an actions.

```javascript
addPet = (pet) => {};
```

```javascript
  constructor() {
    makeObservable(this, {
      pets: observable,
      handleAdopt: action,
      addPet: action,
    });
  }
```

2. It takes an argument with our pet data but we still need an `id`, can you genereta an `id`?

```javascript
addPet = (pet) => {
  pet.id = this.pets[this.pets.length - 1].id + 1;
  this.pets.push(pet);
};
```

3. In `PetCreateModal.js` import our store and in `handleSubmit` call our new method and pass it our state.

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  petStore.addPet(pet);
  handleClose();
};
```

### Updating a pet data.

1. Create a file called `PetUpdateModal.js` in `Components`
2. Copy the same modal from your `PetCreateModal` component and fix the namings.
3. Import the update modal in your `PetItem` component below the adopt button.

```javascript
<button type="button" class="btn btn-info" onClick={() => petStore.handleAdopt(pet.id)}>
Adopt
</button>
<PetUpdateModal />
```

4. For updating, we need the old values for the user to see, so let's pass the old data from `PetItem` to our modal using props.

```javascript
<PetUpdateModal pet={pet} />
```

5. In our update modal get those props to create an inital value for our state, also add an id value because we already have one. but how to show them in our fields? hint: use the `value` property.

```javascript
function PetUpdateModal({pet}) {
  const [show, setShow] = useState(false);
  const [petForm, setPetForm] = useState({
    id: pet.id,
    name: pet.name,
    type: pet.type,
    image: pet.image,
  });
...
```

```javascript
 <Form.Group className="mb-3">
              <Form.Label>Pet name</Form.Label>
              <Form.Control
                onChange={handleChange}
                name="name"
                type="text"
                value={petForm.name}
                placeholder="Pet name"
              />
            </Form.Group>
            <Form.Group className="mb-3">
              <Form.Label>Pet type</Form.Label>
              <Form.Control
                onChange={handleChange}
                name="type"
                value={petForm.type}
                type="text"
                placeholder="Pet type"
              />
            </Form.Group>
            <Form.Group className="mb-3">
              <Form.Label>Pet image</Form.Label>
              <Form.Control
                onChange={handleChange}
                name="image"
                value={petForm.image}
                type="text"
                placeholder="Pet image url"
              />
            </Form.Group>
```

6. In our mobx store create an `update` method thats takes an arguemnt and replaces the old pet with the new data coming with the same `id`. Don't forget to mark it as an `action`.

```javascript
updatePet = (updatedPet) => {
  this.pets = this.pets.map((pet) =>
    pet.id === updatedPet.id ? updatedPet : pet
  );
};
```

7. In `PetUpdateModal` call this new method in the `handleSubmit` function.

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  petStore.updatePet(pet);
  handleClose();
};
```
