<template>
    <div class="camera-container">
        <h1>와 화상통화!</h1>
        <video autoplay></video>
        <video autoplay></video>

        <div class="option-container">
            <div class="option-navigator">
                <select name="" id="">
                    <option
                        v-for="(video, index) in config.devices.videoinput"
                        :value="video.deviceId"
                    >
                        {{ video.label }}
                    </option>
                </select>

                <select name="" id="">
                    <option
                        v-for="(audioin, index) in config.devices.audioinput"
                        :value="audioin.deviceId"
                    >
                        {{ audioin.label }}
                    </option>
                </select>

                <select name="" id="">
                    <option
                        v-for="(audioout, index) in config.devices.audiooutput"
                        :value="audioout.deviceId"
                    >
                        {{ audioout.label }}
                    </option>
                </select>
            </div>
            <div class="option-navigator"></div>
            <!-- <div
                v-for="(config, index) in Object.entries(navConfig)"
                :key="index"
                class="option-navigator"
            >
                {{ Object.entries(config) }}
            </div> -->
        </div>
    </div>
</template>

<script setup>
import { reactive, ref, computed, onMounted } from "vue";
import {
    faVideo,
    faMicrophone,
    faEllipsisVertical,
} from "@fortawesome/free-solid-svg-icons";
import VideoNavigationVue from "./components/VideoNavigation.vue";

const initDeviceConfig = {
    audioinput: [],
    audiooutput: [],
    videoinput: [],
};

const config = reactive({
    devices: initDeviceConfig,
});

const navConfig = reactive({
    current: {
        cam: {
            list: [],
            power: "on",
        },
        mic: {
            list: [],
            power: "on",
        },
    },
    other: {
        cam: {
            list: [],
            power: "on",
        },
        mic: {
            list: [],
            power: "on",
        },
    },
});

async function updateDeviceList() {
    config.devices = initDeviceConfig;

    const currentDevices = await navigator.mediaDevices.enumerateDevices();

    for (let i = 0; i < currentDevices.length; i++) {
        const target = currentDevices[i];
        config.devices[target.kind].push(target);
    }
}

async function getMedia() {
    let stream = null;

    try {
        stream = await navigator.mediaDevices.getUserMedia({
            video: true,
            audio: true,
        });

        updateDeviceList();

        // const peerConnection = new RTCPeerConnection();

        // stream.getTracks().forEach((track) => {
        //     peerConnection.addTrack(track, stream);
        // });
        // console.log("tracks :", stream.getTracks());

        // peerConnection.ondatachannel = (event) => {
        //     const dataChannel = event.channel;

        //     dataChannel.onmessage = (message) => {
        //         console.log("Received message: ", message);
        //     };
        // };

        // const dataChannel = peerConnection.createDataChannel("myDataChannel");

        // dataChannel.onopen = () => {
        //     console.log("Data channel opened");
        // };

        // dataChannel.onclose = () => {
        //     console.log("Data channel closed");
        // };

        // peerConnection
        //     .createOffer()
        //     .then((offer) => {
        //         return peerConnection.setLocalDescription(offer);
        //     })
        //     .then(() => {
        //         // send the offer to the remote peer
        //         const offerData = {
        //             type: "offer",
        //             sdp: peerConnection.localDescription.sdp,
        //         };
        //         console.log("sendOffer :", offerData);
        //         // sendOfferToRemotePeer(offerData);
        //     })
        //     .catch((error) => {
        //         console.error(`Error creating offer: ${error}`);
        //     });

        // function handleOfferFromRemotePeer(offer) {
        //     peerConnection
        //         .setRemoteDescription(new RTCSessionDescription(offer))
        //         .then(() => {
        //             return peerConnection.createAnswer();
        //         })
        //         .then((answer) => {
        //             return peerConnection.setLocalDescription(answer);
        //         })
        //         .then(() => {
        //             const answerData = {
        //                 type: "answer",
        //                 sdp: peerConnection.localDescription.sdp,
        //             };
        //             console.log("sendAnswer :", answerData);
        //             // sendAnswerToRemotePeer(answerData);
        //         })
        //         .catch((error) => {
        //             console.error(`Error creating answer: ${error}`);
        //         });
        // }

        // // handle incoming answers from the remote peer
        // function handleAnswerFromRemotePeer(answer) {
        //     peerConnection
        //         .setRemoteDescription(new RTCSessionDescription(answer))
        //         .then(() => {
        //             console.log("Remote peer accepted the answer");
        //         })
        //         .catch((error) => {
        //             console.error(`Error setting remote description: ${error}`);
        //         });
        // }

        // peerConnection.onicecandidate = (event) => {
        //     if (event.candidate) {
        //         const candidateData = {
        //             type: "candidate",
        //             candidate: event.candidate,
        //         };
        //         console.log("sendCandidate :", candidateData);
        //         // sendCandidateToRemotePeer(candidateData);
        //     }
        // };

        // function handleCandidateFromRemotePeer(candidate) {
        //     peerConnection
        //         .addIceCandidate(new RTCIceCandidate(candidate))
        //         .then(() => {
        //             console.log("Remote peer accepted the ICE candidate");
        //         })
        //         .catch((error) => {
        //             console.error(`Error adding ICE candidate: ${error}`);
        //         });
        // }
    } catch (error) {
        if (error.message === "Permission denied") {
            alert("카메라 및 마이크 권한을 허용해주세요.");
        }
    }
}

onMounted(() => {
    getMedia();

    navigator.mediaDevices.ondevicechange = (event) => {
        updateDeviceList();
    };
});
</script>

<style lang="scss">
.camera-container {
    display: grid;
    margin: 3rem 0;

    h1 {
        text-align: center;
        grid-row: 1/2;
        grid-column: 1/3;
    }

    video {
        width: 100%;
        grid-row: 2/3;
    }

    .option-container {
        display: flex;
        grid-row: 3/4;
        grid-column: 1/3;
        .option-navigator {
            width: 50%;
            display: flex;
            justify-content: space-evenly;
        }
    }
}
</style>
