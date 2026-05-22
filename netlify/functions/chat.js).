export default async function handler(req, res) {
  if (req.method !== 'POST') return res.status(405).json({ error: 'Method not allowed' });
  const { system, messages } = req.body;
  if (!system || !messages) return res.status(400).json({ error: 'Missing fields' });
  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': process.env.ANTHROPIC_API_KEY,
        'anthropic-version': '2023-06-01'
      },
      body: JSON.stringify({ model: 'claude-sonnet-4-20250514', max_tokens: 500, system, messages })
    });
    if (!response.ok) { const e=await response.json(); return res.status(response.status).json({ error: e.error?.message||'API error' }); }
    const data = await response.json();
    const reply = data.content?.[0]?.text;
    if (!reply) return res.status(500).json({ error: 'Empty response' });
    return res.status(200).json({ reply });
  } catch(err) {
    return res.status(500).json({ error: 'Server error' });
  }
}
