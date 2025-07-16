# 🏭 Warehouse Optimization System

## Overview

This advanced n8n workflow analyzes warehouse layouts and inventory data to provide intelligent optimization recommendations. It processes PDF warehouse maps and CSV inventory files to identify fast/slow moving items and suggest optimal placement strategies.

## 🎯 Key Features

### 📊 Data Processing
- **PDF Analysis**: Extracts warehouse zones, locations, and dimensions from layout documents
- **CSV Processing**: Analyzes inventory mutation data with order history
- **Smart Routing**: Automatically detects file types and routes to appropriate processors

### 🧠 Analytics Engine
- **Velocity Analysis**: Identifies fast movers (top 20%) and slow movers (bottom 20%)
- **Co-occurrence Analysis**: Finds items frequently ordered together using market basket analysis
- **Zone Optimization**: Strategic placement recommendations based on accessibility needs
- **ABC Classification**: Implements data-driven warehouse categorization

### 📈 Output Generation
- **Optimization Plans**: Detailed recommendations for item placement
- **Professional Reports**: HTML/PDF reports with executive summaries
- **Database Storage**: PostgreSQL integration for historical analysis
- **GitHub Backup**: Automatic version control of optimization reports

## 🚀 Quick Start

### Prerequisites
1. **n8n Instance**: Running n8n with webhook capabilities
2. **PostgreSQL Database**: For storing analysis results
3. **GitHub Repository**: For report backup (optional)
4. **Docker**: If using containerized GitHub MCP server

### Installation
1. Import the workflow JSON into your n8n instance
2. Configure database credentials for PostgreSQL node
3. Set up GitHub API token for backup functionality
4. Activate the workflow

### Database Setup
```sql
CREATE TABLE warehouse_optimization (
    id SERIAL PRIMARY KEY,
    analysis_date TIMESTAMP DEFAULT NOW(),
    fast_movers JSONB,
    slow_movers JSONB,
    item_pairs JSONB,
    recommendations JSONB,
    warehouse_layout JSONB
);
```

## 📝 Usage Instructions

### Webhook Endpoint
```
POST /webhook/warehouse-optimization
Content-Type: multipart/form-data
```

### Required Files

#### 1. Warehouse Layout PDF
Should contain:
- Zone identifiers (Zone A, Zone B, etc.)
- Location codes (A1-A10, Rack B5, etc.)
- Dimensions and distances
- Shipping/receiving areas
- Storage zones

#### 2. Inventory CSV File
Required columns:
```csv
article_id,order_id,quantity,date
SKU001,ORD123,5,2024-01-15
SKU002,ORD123,2,2024-01-15
SKU001,ORD124,3,2024-01-16
```

**Column Descriptions:**
- `article_id`: SKU or item identifier
- `order_id`: Unique order identifier
- `quantity`: Number of items ordered
- `date`: Order date (ISO format preferred)

## 🔧 Configuration

### Node Configuration

#### PostgreSQL Node
```json
{
  "operation": "insert",
  "schema": "public",
  "table": "warehouse_optimization",
  "columns": "analysis_date, fast_movers, slow_movers, item_pairs, recommendations, warehouse_layout"
}
```

#### GitHub Backup Node
```json
{
  "method": "POST",
  "url": "https://api.github.com/repos/YOUR_USERNAME/YOUR_REPO/contents/warehouse-reports/",
  "authentication": "predefinedCredentialType",
  "nodeCredentialType": "githubApi"
}
```

## 📊 Analytics Algorithms

### Velocity Analysis
- **Fast Movers**: Top 20% of items by order frequency
- **Slow Movers**: Bottom 20% of items by order frequency
- **Classification**: Based on order count and recency

### Affinity Analysis
- **Co-occurrence Matrix**: Items ordered together
- **Confidence Scoring**: Frequency of item pairs
- **Recommendation Engine**: Suggests co-location strategies

### Zone Assignment Strategy
```
Fast Movers → Accessible Zones (near shipping/receiving)
Slow Movers → Storage Zones (higher shelves, back areas)
Paired Items → Same zones for picking efficiency
```

## 📈 Business Impact

### Operational Benefits
- **Picking Efficiency**: 15-30% reduction in travel time
- **Space Optimization**: Maximizes prime real estate usage
- **Labor Cost Reduction**: Optimized picking routes
- **Inventory Accuracy**: Data-driven placement decisions

### Key Metrics
- **Order Fulfillment Speed**: Faster picking of popular items
- **Warehouse Utilization**: Better space allocation
- **Error Reduction**: Logical item groupings
- **Scalability**: Adaptable to changing inventory patterns

## 🔍 Sample Output

### Optimization Report Structure
```json
{
  "optimization_plan": {
    "fast_mover_zones": [
      {
        "article": "SKU001",
        "recommended_zone": "Zone A",
        "priority": "HIGH",
        "reason": "Fast mover (45 orders)",
        "accessibility": "Easy access required"
      }
    ],
    "slow_mover_zones": [...],
    "paired_item_locations": [...],
    "recommendations": [...]
  },
  "summary": {
    "total_items_analyzed": 150,
    "fast_movers_count": 30,
    "slow_movers_count": 30,
    "paired_items_count": 25
  }
}
```

## 🛠️ Troubleshooting

### Common Issues

#### PDF Processing
- **Issue**: No locations extracted
- **Solution**: Ensure PDF contains text (not just images)
- **Check**: Location patterns match regex: `[A-Z]\d{1,3}-[A-Z]\d{1,3}`

#### CSV Analysis
- **Issue**: No fast/slow movers identified
- **Solution**: Verify CSV has required columns
- **Check**: Date format is parseable

#### Database Connection
- **Issue**: Insert failures
- **Solution**: Verify PostgreSQL credentials and table exists
- **Check**: JSONB columns can handle the data structure

### Debug Mode
Enable debug logging in the Code nodes to see:
- Extracted PDF text
- Parsed CSV data
- Generated recommendations

## 🔄 Workflow Nodes

1. **File Upload Webhook** - Receives PDF/CSV files
2. **Check File Type** - Routes based on content type
3. **Read Warehouse PDF** - Extracts text from PDF
4. **Parse CSV Data** - Processes inventory data
5. **Extract Warehouse Layout** - Analyzes PDF content
6. **Analyze Inventory Data** - Performs velocity analysis
7. **Merge Analysis Data** - Combines PDF and CSV insights
8. **Generate Optimization Plan** - Creates recommendations
9. **Save Analysis to Database** - Stores results
10. **Generate Optimization Report** - Creates HTML report
11. **Convert Report to PDF** - Professional formatting
12. **Save to GitHub** - Backup and version control
13. **Send Response** - Returns success confirmation

## 📚 Advanced Usage

### Custom Velocity Thresholds
Modify the velocity analysis code to adjust fast/slow mover percentages:
```javascript
const velocityThreshold = {
  fast: 0.7,  // Top 30% are fast movers
  slow: 0.3   // Bottom 30% are slow movers
};
```

### Additional Analytics
Extend the analysis with:
- Seasonal patterns
- Product categories
- Customer segments
- Supplier lead times

### Integration Options
- **WMS Systems**: Connect to existing warehouse management
- **ERP Integration**: Pull data from enterprise systems
- **BI Tools**: Export to Tableau, Power BI, etc.
- **API Endpoints**: Expose optimization data via REST API

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Machine learning models for demand forecasting
- 3D warehouse visualization
- Real-time optimization updates
- Mobile app for warehouse staff

## 📄 License

MIT License - Feel free to adapt for your warehouse operations!

---

**Created with n8n + MCP Integration**  
*Optimizing warehouses, one workflow at a time* 🚀