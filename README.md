// Next.js API route to send order notification to LINE group

export default async function handler(req, res) { if (req.method !== 'POST') { return res.status(405).json({ error: 'Method not allowed' }); }

const { table, items } = req.body; const lineNotifyToken = process.env.LINE_NOTIFY_TOKEN;

if (!lineNotifyToken) { return res.status(500).json({ error: 'LINE token not configured' }); }

const message = \n\u{1F37D} มีออเดอร์ใหม่จากโต๊ะที่ ${table}\n\nรายการ:\n${items.map(i => - ${i}).join("\n")};

try { const response = await fetch('https://notify-api.line.me/api/notify', { method: 'POST', headers: { 'Content-Type': 'application/x-www-form-urlencoded', Authorization: Bearer ${lineNotifyToken}, }, body: new URLSearchParams({ message }), });

if (!response.ok) {
  const errText = await response.text();
  throw new Error(errText);
}

return res.status(200).json({ success: true });

} catch (error) { return res.status(500).json({ error: error.message }); } }

