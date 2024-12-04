<script setup> 
    const dogImageURL = ref('');
    const dogBreedList = ref([]);
    const dogBreed = ref('');


    const fetchDogImage = async (breed = '') => {
        try {
            const url = breed ? `https://dog.ceo/api/breed/${breed}/images/random` : 'https://dog.ceo/api/breeds/image/random';
            const response = await $fetch(url);
            dogImageURL.value = response.message;
            dogBreed.value = displayedBreed(dogImageURL);
        } catch (error) {
            console.log('Error fetching dog images: ', error);
        }
    };

    fetchDogImage();

    function displayedBreed(dogImageURL) {
        let breed = dogImageURL.value.split('/');
        return breed[4].toUpperCase();
    }
    
    const clicked = async () => {
        await fetchDogImage();
    };

    const fetchDogBreed = async () => {
        try {
            const response = await $fetch('https://dog.ceo/api/breeds/list/all');
            const breeds = response.message;
            let breedList = [];

            for (let breed in breeds) {
                breedList.push(breed);
            }
            dogBreedList.value = breedList;
        } catch (error) {
            console.log('Error fetching dog breed list: ', error);
        }
    };
    fetchDogBreed();

    const clickedBreed = async (breed) => {
        await fetchDogImage(breed);
    }


</script>

<template>
    <main class="main-content" style="justify-content: center; height: 100vh;">
        <div class="container d-flex justify-content-center">
            <Card style="width: 400px" class="mt-3 mb-3 shadow">
                <CardImgTop :src="dogImageURL" />
                <CardBody>
                    <CardTitle>
                        <b-p font-size="2">
                            {{ dogBreed }}
                        </b-p>
                    </CardTitle>
                    <CardText>
                        <b-p text-color="body-secondary" font-size="6">
                            Please click the left button to get a random dog picture, or click the right button to select a dog breed and view its picture.
                        </b-p>
                    </CardText>
                    <div class="d-flex justify-content-center align-items-center gap-3">
                        <b-button color="outline-primary" @click="clicked">Get Random Dog</b-button>
                        <Dropdown>
                            <DropdownToggle color="outline-success">
                                Choose Dog Breed
                            </DropdownToggle>
                            <DropdownMenu class="max-height-200 overflow-auto" style="max-height: 200px; width: 100%; overflow-y: auto;">
                                <DropdownItem v-for="(breed, index) in dogBreedList"
                                            :key = "index"
                                            @click="clickedBreed(breed)">{{ breed }}</DropdownItem>
                            </DropdownMenu>
                        </Dropdown>
                    </div>
                </CardBody>
            </Card>
        </div>
    </main>
</template>