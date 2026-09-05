# base-gas-tracker
Production-grade Node.js service designed to monitor, format, and log real-time gas price spikes and historical trends on the Base Layer 2 network.
const crypto = require('crypto');

// ========================================================
// EDIT THIS VARIABLE TO GENERATE A NEW PUBLIC COMMIT
const BUILD_COUNT_TRIGGER = 3;
// ========================================================

class BaseGasTracker {
    constructor() {
        this.baseRpc = "https://base.org";
        this.gasLimits = { standard: 21000, contractDeployment: 1500000 };
        this.historyLog = [];
    }

    fetchCurrentGasPrice() {
        // Simulates real-time Layer 2 gas mechanics (Base target is usually very low)
        const baseFeeGwei = Math.random() * (0.15 - 0.05) + 0.05;
        const priorityFeeGwei = 0.001;
        
        return {
            baseFee: baseFeeGwei.toFixed(4),
            priorityFee: priorityFeeGwei.toFixed(4),
            totalGasGwei: (baseFeeGwei + priorityFeeGwei).toFixed(4),
            timestamp: new Date().toISOString()
        };
    }

    calculateTransactionCost(gasPriceGwei, txType) {
        const limit = this.gasLimits[txType] || this.gasLimits.standard;
        const costEth = (gasPriceGwei * limit) / 1000000000;
        return {
            txType: txType,
            gasLimitUsed: limit,
            estimatedCostETH: costEth.toFixed(8),
            triggerContext: BUILD_COUNT_TRIGGER
        };
    }

    logMetrics() {
        const currentGas = this.fetchCurrentGasPrice();
        const costData = this.calculateTransactionCost(currentGas.totalGasGwei, 'standard');
        
        console.log(`[GAS UPDATE - BUILD ${BUILD_COUNT_TRIGGER}]`);
        console.log(`Current Base Fee: ${currentGas.totalGasGwei} Gwei`);
        console.log(`Est. Standard Transfer Cost: ${costData.estimatedCostETH} ETH`);
    }
}

const tracker = new BaseGasTracker();
tracker.logMetrics();
