<template>
  <q-page class="relative-position">
    <q-scroll-area class="absolute full-width full-height">
      <div class="q-py-lg q-px-md row items-end q-gutter-sm">
        <div class="col">
          <q-input
            bottom-slots
            v-model="newQweetContent"
            placeholder="What's Happening"
            counter
            maxlength="280"
            autogrow
            class="new-qweet"
          >
            <template v-slot:before>
              <q-avatar size="xl">
                <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
              </q-avatar>
            </template>

            <!-- <template v-slot:after>

          </template> -->
          </q-input>
        </div>
        <div class="col col-shrink">
          <q-btn
            @click="addNewQweet"
            class="q-mb-md"
            unelevated
            rounded
            no-caps
            :disabled="!newQweetContent"
            color="primary"
            label="Qweet"
          />
        </div>
      </div>
      <q-separator size="10px" color="gray-2" class="divider" />

      <q-list separator>
        <transition-group
          appear
          enter-active-class="animated fadeIn slow"
          leave-active-class="animated fadeOut slow"
        >
          <q-item class="q-py-md" v-for="qweet in qweets" :key="qweet.id">
            <q-item-section avatar top>
              <q-avatar size="xl">
                <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
              </q-avatar>
            </q-item-section>

            <q-item-section>
              <q-item-label class="text-subtitle1">
                <strong>Mohamed Adel</strong>
                <span class="text-grey-7">
                  @alalamey
                  <br class="lt-md" />&bull; {{ qweet.date | relativeDate }}
                </span>
              </q-item-label>
              <q-item-label class="qweet-content text-body1">
                {{ qweet.content }}
              </q-item-label>

              <div class="qweet-icons row justify-between q-mt-sm">
                <div v-if="!liked">
                  <q-btn
                    flat
                    round
                    color="grey"
                    icon="far fa-heart"
                    @click="like"
                  />{{ likeCount }}
                </div>

                <div v-if="liked">
                  <q-btn
                    flat
                    round
                    color="pink"
                    icon="fas fa-heart"
                    @click="unlike"
                  />{{ likeCount }}
                </div>

                <q-btn flat round color="grey" icon="far fa-comment" />
                <q-btn flat round color="grey" icon="fas fa-retweet" />
                <q-btn
                  @click="deleteQweet(qweet)"
                  flat
                  round
                  color="grey"
                  icon="fas fa-trash"
                />
              </div>
            </q-item-section>
          </q-item>
        </transition-group>

        <q-separator inset="item" />
      </q-list>
    </q-scroll-area>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { formatDistance } from "date-fns";

export default defineComponent({
  name: "PageHome",
  data() {
    return {
      likeCount: 0,
      id: "",
      newQweetContent: "",
      qweets: [
        {
          id: "ID1",
          content:
            "Quozio is the fastest way to create quotes to save and share. Making beautiful quotes just became easy! Try it yourself to see why millions choose Quozio as ",
          date: 1634590515606,
          liked: false,
        },
        {
          id: "ID2",
          content:
            "Quozio is the fastest way to create quotes to save and share. Making beautiful quotes just became easy! Try it yourself to see why millions choose Quozio as ",
          date: 1634590521472,
          liked: false,
        },
      ],
    };
  },
  // filters: {
  //   relativeDate(value) {
  //     return formatDistance(value, new Date());
  //   },
  // },
  methods: {
    addNewQweet() {
      let newQweet = {
        content: this.newQweetContent,
        date: Date.now(),
        liked: false,
      };
      this.qweets.unshift(newQweet);
      this.newQweetContent = "";
    },

    deleteQweet(qweet) {
      let dateToDelete = qweet.date;
      let index = this.qweets.findIndex((qweet) => qweet.date === dateToDelete);
      console.log(index);
      this.qweets.splice(index, 1);
    },

    like() {
      
      this.liked = true;
      this.likeCount++;
    },

    unlike() {
      this.liked = false;
      this.likeCount--;
    },

    relativeDate(value) {
      return formatDistance(value, new Date());
    },
  },
});
</script>

<style>
.new-qweet textarea {
  font-size: 19px;
  line-height: 1.4 !important;
}

.divider {
  border-top: 1px solid;
  border-bottom: 1px solid;
  border-color: grey-4;
}

.qweet-content {
  white-space: pre-line;
}

.qweet:not(:first-child) {
  border-top: 1px solid rgba(0, 0, 0, 0.12);
}

.qweet-icons {
  margin-left: -5px;
}
</style>
