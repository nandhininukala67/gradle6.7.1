public class MerchantPayoutManager extends DBManager<MerchantPayoutDetails> {
    
    public MerchantPayoutManager(DB db) {
        super(db, MerchantPayoutDetails.class);
    }

    public MerchantPayoutDetails get(MerchantDetails merchant) {
        return getItemByParam("mid", merchant.getMid());
    }

    public List<MerchantPayoutDetails> getMerchantPayoutTransactionList(Date date, int offset, int limit) {
        Query<MerchantPayoutDetails> query = db.session().createQuery(
                "from " + MerchantPayoutDetails.class.getCanonicalName() + 
                " where status=:status and created >:fromDate"
        );
        query.setParameter("status", "A");
        query.setParameter("fromDate", date);
        query.setFirstResult(offset);
        query.setMaxResults(limit);
        return query.list();
    }

    public List<UpiTxnPayoutDetails> getUpiPayoutTransactionList() {
        Query<UpiTxnPayoutDetails> query = db.session().createQuery(
                "from " + UpiTxnPayoutDetails.class.getCanonicalName() + " where status=:status"
        );
        query.setParameter("status", "A");
        return query.list();
    }

    public MerchantPayoutDetails getMerchantPayoutById(Long id) {
        Query<MerchantPayoutDetails> query = db.session().createQuery(
                "from " + MerchantPayoutDetails.class.getCanonicalName() + " where id=:id"
        );
        query.setParameter("id", id);
        List<MerchantPayoutDetails> merchantPayoutDetails = query.list();
        return merchantPayoutDetails.isEmpty() ? null : merchantPayoutDetails.get(0);
    }

    public MerchantPayoutDetails getMerchantPayoutBySwitchTxnId(String switchTxnId) {
        Query<MerchantPayoutDetails> query = db.session().createQuery(
                "from " + MerchantPayoutDetails.class.getCanonicalName() + " where switchTxnId=:switchTxnId"
        );
        query.setParameter("switchTxnId", switchTxnId);
        return query.uniqueResult();
    }
}

