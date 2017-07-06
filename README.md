# C306
Devoir 3 C306 ingénierie logicielle
/**
 *
 * @author BLU Komi Mokpokpo
 */
public interface ISolveur {

    /**
     * Teste si le puzzle est un puzzle valide.
     *
     * @return boolean
     */
    boolean verifierPuzzle();

    /**
     * RÃ©soud le puzzle passÃ© en paramÃ¨tre.
     *
     * @throws IllegalArgumentException si le puzzle Ã  rÃ©soudre n'est pas
     * valable ou si aucune solution n'a pu Ãªtre calculÃ©e
     * @return une grille bien remplie
     */
    boolean resoudre();

    /**
     * Affiche la solution trouvÃ©e au puzzle.
     */
    void afficherSolution();
package com.github.sudoku;

import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertTrue;
import static org.junit.Assert.assertFalse;
import org.junit.BeforeClass;
import org.junit.Test;

/**
 * Test de la classe GrilleImpl.java.
 *
 * @author yeboah_jp@yahoo.fr (Blu Komi Mokpokpo)
 */
public final class TestGrille {

    /**
     * grille un objet de la classe.
     *
     */
    private static GrilleImpl grille;

    /**
     * initialise la classe.
     *
     */
    @BeforeClass
    public static void initialize() {
        final char[][] sudoku
                = {{'@', '3', '@', '6', '7', '@', '9', '@', '2'},
                {'6', '7', '2', '1', '9', '5', '3', '4', '8'},
                {'1', '9', '@', '@', '@', '@', '5', '6', '7'},
                {'@', '5', '@', '7', '6', '1', '@', '2', '3'},
                {'@', '2', '6', '@', '@', '@', '7', '9', '@'},
                {'@', '1', '3', '9', '@', '4', '8', '5', '6'},
                {'@', '6', '@', '5', '3', '@', '@', '8', '@'},
                {'2', '8', '7', '@', '1', '9', '@', '3', '5'},
                {'3', '4', '5', '2', '8', '6', '1', '7', '9'}};
        grille = new GrilleImpl(sudoku);
    }

    /**
     * Test de la mÃ©thode getDimension().
     *
     */
    @Test
    public void testDimension() {
        final int dim = 9;
        assertEquals(dim, grille.getDimension());
    }

    /**
     * Test de la mÃ©thode setValue().
     *
     */
    @Test
    public void testSetValue() {
        final char value = '8';
        final int x = 0;
        final int y = 5;
        grille.setValue(x, y, value);
    }

    /**
     * Test de la mÃ©thode setValue(). x est hors bornes
     */
    @Test(expected = IllegalArgumentException.class)
    public void testSetValueHorsBorne() {
        final char value = '8';
        final int x = 10;
        final int y = 5;
        grille.setValue(x, y, value);
    }

    /**
     * Test de la mÃ©thode setValue(). value est incorrect
     */
    @Test(expected = IllegalArgumentException.class)
    public void testSetValueIncorrect() {
        final char value = 'g';
        final int x = 0;
        final int y = 5;
        grille.setValue(x, y, value);
    }

    /**
     * Test de la mÃ©thode setValue(). value non autorisÃ©e dans la grille
     */
    @Test(expected = IllegalArgumentException.class)
    public void testSetValueNonPossible() {
        char value = '5';
        final int x = 3;
        final int y = 0;
        grille.setValue(x, y, value);
    }

    /**
     * Test de la mÃ©thode getValue().
     *
     */
    @Test
    public void testGetValue() {
        final char value = '9';
        final int x = 1;
        final int y = 4;
        assertEquals(value, grille.getValue(x, y));
    }

    /**
     * Test de la mÃ©thode complete().
     *
     */
    @Test
    public void testComplete() {
        char[][] completeSudoku
                = {{'5', '3', '4', '6', '7', '8', '9', '1', '2'},
                {'6', '7', '2', '1', '9', '5', '3', '4', '8'},
                {'1', '9', '8', '3', '4', '2', '5', '6', '7'},
                {'8', '5', '9', '7', '6', '1', '4', '2', '3'},
                {'4', '2', '6', '8', '5', '3', '7', '9', '1'},
                {'7', '1', '3', '9', '2', '4', '8', '5', '6'},
                {'9', '6', '1', '5', '3', '7', '2', '8', '4'},
                {'2', '8', '7', '4', '1', '9', '6', '3', '5'},
                {'3', '4', '5', '2', '8', '6', '1', '7', '9'}};
        GrilleImpl gi = new GrilleImpl(completeSudoku);
        assertTrue(gi.complete());
    }

    /**
     * Test de la mÃ©thode complete(). avec un tableau incomplet
     */
    @Test
    public void testNonComplete() {
        char[][] completeSudoku
                = {{'5', '3', '4', '6', '7', '8', '9', '1', '2'},
                {'6', '7', '2', '1', '9', '5', '3', '4', '8'},
                {'1', '9', '@', '3', '4', '2', '5', '6', '7'},
                {'8', '5', '9', '7', '6', '1', '4', '2', '3'},
                {'4', '2', '6', '8', '@', '3', '7', '9', '1'},
                {'7', '1', '3', '9', '2', '4', '8', '5', '6'},
                {'9', '6', '1', '5', '3', '7', '2', '8', '4'},
                {'2', '8', '7', '4', '1', '9', '6', '3', '5'},
                {'3', '4', '5', '2', '8', '6', '1', '7', '9'}};
        GrilleImpl gi = new GrilleImpl(completeSudoku);
        assertFalse(gi.complete());
    }

    /**
     * Test de la mÃ©thode possible(). avec une valeur hors bornes
     */
    @Test(expected = IllegalArgumentException.class)
    public void testHorsBornesWithException() {
        final int dim = 9;
        final int x = 20;
        final int y = 20;
        grille.horsBornes(x, y, dim);
    }
}

}
package com.github.sudoku;

/**
 *
 * @author BLU Komi Mokpokpo
 */
public class GrilleImpl implements Grille {

    /**
     * sudoku une grille de sudoku 9x9.
     */
    private char[][] sudoku;

    /**
     * constructeur de la grille.
     * @param plateau char[][]
     */
    public GrilleImpl(final char[][] plateau) {
        this.sudoku = plateau;
    }

    /**
     * constructeur par dÃ©faut de la grille.
     */
    public GrilleImpl() {

    }

    @Override
    public final int getDimension() {
        return this.sudoku.length;
    }

    /**
     * Accesseur de la variable sudoku.
     *
     * @return char[][]
     */
    public final char[][] getSudoku() {
        return sudoku;
    }

    /**
     * Modifie la variable sudoku.
     *
     * @param sudoku1 char[][]
     */
    public final void setSudoku(final char[][] sudoku1) {
        this.sudoku = sudoku1;
    }

    @Override
    public final void setValue(final int x, final int y, final char value)
            throws IllegalArgumentException {
        if (possible(x, y, value)) {
            sudoku[x][y] = value;
        } else {
            throw new IllegalArgumentException("La valeur est interdite au vu "
                    + "des autres valeurs de la grille");
        }
    }

    @Override
    public final char getValue(final int x, final int y)
            throws IllegalArgumentException {
        int dim = getDimension();
        horsBornes(x, y, dim);
        return sudoku[x][y];
    }

    @Override
    public final boolean complete() {
        int dim = getDimension();
        for (int i = 0; i < dim; i++) {
            for (int j = 0; j < dim; j++) {
                if (sudoku[i][i] == EMPTY) {
                    return false;
                }
            }
        }
        return true;
    }

    @Override
    public final boolean possible(final int x, final int y, final char value)
            throws IllegalArgumentException {
        int dim = getDimension();
        horsBornes(x, y, dim);
        //Test si la valeur est autorisÃ©e
        if (!contient(dim, value)) {
            throw new IllegalArgumentException("Ce caractÃ¨re "
                    + "n'est pas autorisÃ©");
        }

        char[] ligne = sudoku[x]; //Valeur de la ligne i de la grille
        char[] colonne = new char[dim]; //Valeurs de la colonne j de la grille
        //Remplissage des valeurs de la colonne Y
        for (int i = 0; i < dim; i++) {
            colonne[i] = sudoku[i][y];
        }

        /*
         * Test si la valeur est prÃ©sente une seule fois sur la ligne
         * et une seule fois sur la colonne
         */
        if (nombreOccurences(ligne, value) > 1
                || nombreOccurences(colonne, value) > 1) {
            return false;
        }

        return testCarre(x, y, value);
    }

    /**
     * Calcul le nombre d'occurrences d'une valeur dans un tableau.
     *
     * @param tableau char[] un tableau donnÃ©es de caractÃ¨res
     * @param value un caractÃ¨re
     * @return int le nombre
     */
    public final int nombreOccurences(final char[] tableau, final char value) {
        int nbOccurences = 0;

        for (char c : tableau) {
            if (c == value) {
                nbOccurences++;
            }
        }
        return nbOccurences;
    }

    /**
     * VÃ©rifie si le caractÃ¨re est autorisÃ©.
     *
     * @param taille un tableau donnÃ©es de caractÃ¨res
     * @param value un caractÃ¨re
     * @return true si le caractÃ¨re est autorisÃ© et false sinon
     */
    private boolean contient(final int taille, final char value) {
        for (int i = 0; i < taille; i++) {
            if (POSSIBLE[i] == value) {
                return true;
            }
        }
        return value == EMPTY;
    }

    /**
     * Renvoie un tableau reprÃ©sentant un carrÃ© de la grille.
     *
     * @param x position x dans la grille
     * @param y position y dans la grille
     * @param value un caractÃ¨re
     * @return char[][] un carrÃ© de la grille
     */
    public final boolean testCarre(final int x, final int y, final char value) {
        //Taille d'un carre aprÃ¨s dÃ©coupage de la grille en carrÃ©s
        int dimCarre = (int) Math.sqrt(Double.valueOf(getDimension()));
        char[][] carre = new char[dimCarre][dimCarre];
        //Position X selon le dÃ©coupage de la grille en carrÃ©s
        int xx = x / dimCarre;
        //Position Y selon le dÃ©coupage de la grille en carrÃ©s
        int yy = y / dimCarre;
        int infX = dimCarre * xx, supX = dimCarre * (1 + xx);
        int infY = dimCarre * yy, supY = dimCarre * (1 + yy);

        for (int i = infX; i < supX; i++) {
            for (int j = infY; j < supY; j++) {
                if (sudoku[i][j] == value) {
                    return false;
                }
            }
        }
        return true;
    }

    /**
     * Test si x ou y sont hors bornes.
     *
     * @param x index de la ligne
     * @param y index de la colonne
     * @param dim dimension du tableau
     * @throws IllegalArgumentException si x > dim
     * @throws IllegalArgumentException si y > dim
     */
    public final void horsBornes(final int x, final int y, final int dim)
            throws IllegalArgumentException {
        if (x > dim || y > dim) {
            throw new IllegalArgumentException("x et y sont "
                    + "hors bornes 0-8 ");
        }
    }

}
