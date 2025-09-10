<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF to Kannada Spreadsheet Converter</title>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .lucide { width: 20px; height: 20px; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        // Simple Lucide icons replacement
        const Upload = () => <span className="lucide">📤</span>;
        const Download = () => <span className="lucide">📥</span>;
        const FileText = () => <span className="lucide">📄</span>;

        const PDFProcessor = () => {
            const [manualInput, setManualInput] = useState('');
            const [processedData, setProcessedData] = useState([]);
            const [loading, setLoading] = useState(false);
            const [error, setError] = useState('');

            const kannadaHeaders = [
                'ಕ್ರಮ ಸಂಖ್ಯೆ',
                'ಮನೆ ಸಂಖ್ಯೆ', 
                'ಮತದಾರರ ಹೆಸರು',
                'ಸಂಬಂಧ',
                'ಸಂಬಂಧಿಯ ಹೆಸರು',
                'ಲಿಂಗ',
                'ವಯಸ್ಸು',
                'ಮತದಾರರ ಗುರುತಿನ ಚೀಟಿ ಸಂಖ್ಯೆ'
            ];

            const processManualInput = () => {
                if (!manualInput.trim()) {
                    setError('Please enter some data to process');
                    return;
                }

                try {
                    setLoading(true);
                    const lines = manualInput.split('\n').filter(line => line.trim());
                    const processedRows = [];

                    lines.forEach((line, index) => {
                        let parts = [];
                        if (line.includes('|')) {
                            parts = line.split('|');
                        } else if (line.includes('\t')) {
                            parts = line.split('\t');
                        } else if (line.includes(',')) {
                            parts = line.split(',');
                        } else {
                            parts = line.trim().split(/\s+/);
                        }

                        if (parts.length >= 4) {
                            processedRows.push({
                                [kannadaHeaders[0]]: parts[0]?.trim() || '',
                                [kannadaHeaders[1]]: parts[1]?.trim() || '',
                                [kannadaHeaders[2]]: parts[2]?.trim() || '',
                                [kannadaHeaders[3]]: parts[3]?.trim() || '',
                                [kannadaHeaders[4]]: parts[4]?.trim() || '',
                                [kannadaHeaders[5]]: parts[5]?.trim() || '',
                                [kannadaHeaders[6]]: parts[6]?.trim() || '',
                                [kannadaHeaders[7]]: parts[7]?.trim() || '',
                            });
                        }
                    });

                    setProcessedData(processedRows);
                    setError('');
                } catch (err) {
                    setError('Error processing data: ' + err.message);
                } finally {
                    setLoading(false);
                }
            };

            const copyToClipboard = () => {
                const headers = kannadaHeaders.join('\t');
                const rows = processedData.map(row => 
                    kannadaHeaders.map(header => row[header] || '').join('\t')
                );
                
                const fullData = [headers, ...rows].join('\n');
                
                navigator.clipboard.writeText(fullData).then(() => {
                    alert('ಡೇಟಾ ಕ್ಲಿಪ್‌ಬೋರ್ಡ್‌ಗೆ ಕಾಪಿ ಮಾಡಲಾಗಿದೆ! (Data copied to clipboard!)');
                });
            };

            return (
                <div className="max-w-6xl mx-auto p-6 bg-white min-h-screen">
                    <div className="mb-6">
                        <h1 className="text-3xl font-bold text-gray-800 mb-2">PDF to Kannada Spreadsheet Converter</h1>
                        <p className="text-gray-600">Convert voter list data to Kannada format for spreadsheet</p>
                    </div>

                    {loading && (
                        <div className="flex items-center justify-center p-8">
                            <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
                            <span className="ml-2">Processing data...</span>
                        </div>
                    )}

                    {error && (
                        <div className="bg-red-50 border border-red-200 rounded-lg p-4 mb-4">
                            <p className="text-red-800">{error}</p>
                        </div>
                    )}

                    {/* Manual Input Section */}
                    <div className="border rounded-lg p-4 mb-6">
                        <h3 className="font-semibold mb-3 flex items-center text-lg">
                            <Upload /> 
                            <span className="ml-2">Data Input</span>
                        </h3>
                        <div className="space-y-4">
                            <div>
                                <label className="block text-sm font-medium text-gray-700 mb-2">
                                    Paste your OCR data here (supports various separators: |, tab, comma, space)
                                </label>
                                <textarea
                                    value={manualInput}
                                    onChange={(e) => setManualInput(e.target.value)}
                                    placeholder={`Example formats:
1|123/A|राम कुमार|पुत्र|श्याम कुमार|M|25|ABCD1234567
2|124/B|सुनीता देवी|पत्नी|राम कुमार|F|23|EFGH2345678

Or use tabs, commas, or spaces as separators.`}
                                    className="w-full h-48 p-3 border border-gray-300 rounded-lg text-sm font-mono"
                                />
                            </div>
                            <div className="flex space-x-3">
                                <button
                                    onClick={processManualInput}
                                    className="bg-green-600 text-white px-6 py-2 rounded-lg hover:bg-green-700 flex items-center"
                                    disabled={loading}
                                >
                                    <span className="mr-2">⚡</span>
                                    Process Data
                                </button>
                                <button
                                    onClick={() => setManualInput('')}
                                    className="bg-gray-500 text-white px-6 py-2 rounded-lg hover:bg-gray-600"
                                >
                                    Clear
                                </button>
                            </div>
                        </div>
                    </div>

                    {/* Processed Data Preview */}
                    {processedData.length > 0 && (
                        <div className="border rounded-lg p-4 mb-6">
                            <h3 className="font-semibold mb-3 text-lg">Processed Data ({processedData.length} rows)</h3>
                            <div className="bg-gray-50 p-3 rounded max-h-60 overflow-auto">
                                <table className="w-full text-xs border-collapse">
                                    <thead>
                                        <tr className="border-b bg-blue-50">
                                            {kannadaHeaders.map((header, index) => (
                                                <th key={index} className="border-r px-2 py-1 text-left font-medium">
                                                    {header}
                                                </th>
                                            ))}
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {processedData.slice(0, 10).map((row, index) => (
                                            <tr key={index} className="border-b hover:bg-gray-50">
                                                {kannadaHeaders.map((header, colIndex) => (
                                                    <td key={colIndex} className="border-r px-2 py-1">
                                                        {row[header] || ''}
                                                    </td>
                                                ))}
                                            </tr>
                                        ))}
                                        {processedData.length > 10 && (
                                            <tr>
                                                <td colSpan={kannadaHeaders.length} className="px-2 py-1 text-center text-gray-500">
                                                    ... and {processedData.length - 10} more rows
                                                </td>
                                            </tr>
                                        )}
                                    </tbody>
                                </table>
                            </div>
                            <div className="mt-4">
                                <button
                                    onClick={copyToClipboard}
                                    className="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 flex items-center"
                                >
                                    <Download />
                                    <span className="ml-2">Copy for Spreadsheet</span>
                                </button>
                            </div>
                        </div>
                    )}

                    {/* Kannada Headers Reference */}
                    <div className="border rounded-lg p-4">
                        <h3 className="font-semibold mb-3 text-lg">Required Kannada Headers</h3>
                        <div className="grid md:grid-cols-2 gap-2">
                            {kannadaHeaders.map((header, index) => (
                                <div key={index} className="bg-blue-50 p-2 rounded text-sm">
                                    <span className="font-medium">{index + 1}.</span> {header}
                                </div>
                            ))}
                        </div>
                    </div>
                </div>
            );
        };

        ReactDOM.render(<PDFProcessor />, document.getElementById('root'));
    </script>
</body>
</html>
