# Ultimate Dog Image Fetcher

We've already used the [Dog API](https://dog.ceo/dog-api/) to get the image of a single, completely random dog. However, today we're doing something a little different. Today, we are creating a full-fledged app to fulfill all of your dog-image-searching desires.

This app, naturally, will have _routes_.

## Part 1: Route Structure and How This App Is Different

We've been teaching you the method where you have been **centralizing state** and passing it down to child components via props. That's good! However, this app is a little different.

We are making the conscious decision that we **don't want** to centralize state. Instead of one big app, our app is going to be more like a couple of tiny apps inside different routes.

These apps will be:

- **Random dog image** `/random` - We've done this one before. Load a random dog image. Include a button that loads a new random image when clicked.
  - **Multiple random dogs** `/random/:num` - This will render the number of random dog images requested for in the `num` URL parameter. No button required.
  - _Bonus: Extend the functionality of `/random` with a `select` box to select the number of dog images rendered. If the box is selected, it should take the user to the route in `/random/:num`, where `num` is the selected number._
- **Random dog of specific breed** `/randomBreed` - This one is a little different. Initially, the user will not see a dog image. There will be a `select` box, which will contain a list of **all dog breeds** contained in the Dog API (_note: don't wory about sub-breeds for now_). There will be a button next to that `select`, which, when clicked, will fetch a random image of a dog with the selected breed.
  - Luckily, the Dog API has a [special route](https://dog.ceo/dog-api/documentation/breed) to handle these kinds of requests.

These routes will each **keep their own state**. They don't have to interact with each other, because (theoretically) a user looking for a picture of a bulldog does not need to see a picture of a random dog.

**If we are making this decision well in advance as a deliberate architectural move, it's fine to not have a centralized state.** We should just keep in mind that, should we try to add more granular functionality after the fact, we may be in for a bad time.

## Part 2: Component Structure

Alright. So given what our routes look like, and what our needs for holding state are, our component structure might look something like this:

- `App` - Setting up routing for our three sub-routes and rendering navbar.
- `Navbar` - Always renders above any other component, rendered by `App`. Holds `Link` references to our three sub-routes.
- `RandomImg` - The component for two sub-routes: `/random` and `/random/:num`, depending on what URL parameters it receives. Renders `Image`s, holding image URLs in state and requesting them from the Dog API.
- `BreedImg` - The component for the `randomBreed` sub-route. Holds state for the user's breed choice, as well as for image URLs to render `Image`s.
- `Image` - A functional component that accepts an image URL as a prop and returns a properly-styled `img` tag.
