const { default: makeWASocket, useMultiFileAuthState } = require("@whiskeysockets/baileys");
const pino = require("pino");

// اسم المطور واسم البوت
const OWNER_NAME = process.env.OWNER_NAME || "عبدو";
const BOT_NAME = process.env.BOT_NAME || "عبدو";

async function startBot() {
  const { state, saveCreds } = await useMultiFileAuthState("session");

  const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true,
    logger: pino({ level: "silent" })
  });

  sock.ev.on("creds.update", saveCreds);

  sock.ev.on("messages.upsert", async ({ messages }) => {
    const msg = messages[0];
    if (!msg.message || msg.key.fromMe) return;

    const text = msg.message.conversation || msg.message.extendedTextMessage?.text;
    const from = msg.key.remoteJid;
    if (!text) return;

    if (text === "ping") {
      await sock.sendMessage(from, { text: "🏓 شغال يا زعيم!" });
    } else if (text === OWNER_NAME) {
      await sock.sendMessage(from, { text: `👑 المطور ${OWNER_NAME} حاضر!` });
    } else if (text === BOT_NAME) {
      await sock.sendMessage(from, { text: `🤖 البوت ${BOT_NAME} جاهز!` });
    }
  });

  console.log("✅ البوت اشتغل.. استنى الـ QR كود!");
}

startBot().catch(err => console.log(err));
