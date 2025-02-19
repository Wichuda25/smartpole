<template>
    <div class="websocket-container">
        <!-- แสดงสถานะการเชื่อมต่อ -->
        <div v-if="connectionStatus" :class="statusClass">
            <h2>{{ connectionStatus }}</h2>
        </div>

        <!-- แสดงข้อมูล VitalSigns -->
        <div v-if="receivedMessage" class="message-box">
            <h3>Vital Signs Data:</h3>
            <div>
                <p><strong>Temperature:</strong> {{ receivedMessage.temperature }} °C</p>
            </div>
            <div>
                <p><strong>Heart Rate (SpO2):</strong> {{ receivedMessage.heartRateSpO2 }} bpm</p>
            </div>
            <div>
                <p><strong>spo2:</strong> {{ receivedMessage.spo2 }}%</p>
            </div>
            <div>
                <p><strong>Systolic Pressure:</strong> {{ receivedMessage.systolic_pressure }} mmHg</p>
            </div>
            <div>
                <p><strong>Diastolic Pressure:</strong> {{ receivedMessage.diastolic_pressure }} mmHg</p>
            </div>
            <div>
                <p><strong>Heart Rate:</strong> {{ receivedMessage.heart_rate }} bpm</p>
            </div>
            <div>
                <p><strong>Weight:</strong> {{ receivedMessage.weight }} kg</p>
            </div>
            <div>
                <p><strong>Height:</strong> {{ receivedMessage.height }} cm</p>
            </div>
            <div>
                <p><strong>Glucose Level:</strong> {{ receivedMessage.glucose }} mg/dL</p>
            </div>
            <div>
                <p><strong>Type:</strong> {{ receivedMessage.type }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            socket: null,
            connectionStatus: '',
            receivedMessage: null,
            statusClass: 'disconnected',
        };
    },
    methods: {
        updateMessage(newData) {
            if (this.receivedMessage) {
                Object.keys(newData).forEach((key) => {
                    if (newData[key] !== null && newData[key] !== undefined) {
                        // ถ้าค่าที่รับมาไม่ใช่ None (null หรือ undefined) ให้อัปเดต
                        this.receivedMessage[key] = newData[key];
                    }
                });
            } else {
                this.receivedMessage = { ...newData };
            }
        },
        connectWebSocket() {
            this.socket = new WebSocket('ws://localhost:8989/ws');

            this.socket.onopen = () => {
                console.log('WebSocket Connected');
                this.connectionStatus = 'Connected';
                this.statusClass = 'connected';
            };

            this.socket.onerror = (error) => {
                console.error('WebSocket Error:', error);
                this.connectionStatus = 'Error';
                this.statusClass = 'error';
            };

            this.socket.onmessage = (event) => {
                try {
                    // รับข้อมูล JSON จาก WebSocket
                    const newMessage = JSON.parse(event.data);
                    this.updateMessage(newMessage);  // ใช้ฟังก์ชันเพื่ออัปเดตข้อมูล
                } catch (error) {
                    console.error('Error parsing message:', error);
                }
            };
        },
    },
    mounted() {
        this.connectWebSocket();
    },
    beforeDestroy() {
        if (this.socket) {
            this.socket.close();
        }
    },
};
</script>

<style scoped>
.websocket-container {
    font-family: Arial, sans-serif;
    margin: 20px;
    padding: 20px;
    border-radius: 8px;
    background-color: #f0f0f0;
    max-width: 600px;
    margin: 0 auto;
    text-align: center;
}

h2 {
    font-size: 1.5em;
    margin-bottom: 20px;
}

.message-box {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.connected {
    color: #4caf50;
}

.error {
    color: #f44336;
}

.disconnected {
    color: #9e9e9e;
}

p {
    margin: 10px 0;
}

strong {
    font-weight: bold;
}
</style>
